We have a problem TF-F074 is complete / Done but did not actually complete correctly

Here's the full end-to-end trace of what happens when a user enters a key for
  llm_nvidia_nim and hits Save.

  ---
  What happens when you save llm_nvidia_nim credentials

  1. Frontend — form schema and submit

  PROVIDER_CREDENTIAL_SCHEMAS["llm_nvidia_nim"] in runtime.ts:526 defines one field: { name: "api_key", label: "API
  Key", secret: true }.

  The field renders as <input type="password">. On submit, handleSave("llm_nvidia_nim") in
  ProviderConfigurationPanel.tsx:69 runs. It:
  - reads PROVIDER_CREDENTIAL_SCHEMAS for llm_nvidia_nim (just api_key)
  - strips empty optional fields (none here — api_key is required)
  - calls updateCredential("llm_nvidia_nim", { api_key: "<typed_value>" })

  2. Frontend → Backend — HTTP call

  updateCredential in runtime.ts:569 sends:
  PUT /admin/credentials/llm_nvidia_nim
  Content-Type: application/json
  { "fields": { "api_key": "<typed_value>" } }

  3. Backend — validation (admin_routes.py:218)

  update_credential handler:
  - confirms "llm_nvidia_nim" is in _PROVIDER_FIELD_NAMES (it's there because LLM_PROVIDER_SECRET_SCHEMAS populates it
  at module load: admin_routes.py:28)
  - confirms api_key is present in the payload
  - credential_type is set to "api_key" (sorted join of field names)

  4. Encryption — key_manager.encrypt_payload() (key_manager.py:42)

  KeyManager.from_environment() reads TRADEFORGE_MASTER_KEY from the OS env and creates a Fernet instance (AES-128-CBC +
   HMAC-SHA256).

  encrypt_payload({"api_key": "<value>"}) → normalizes, JSON-serializes, Fernet-encrypts → returns opaque bytes.

  5. Credential object is built and stored (.keys.enc)

  A Credential dataclass is constructed with:
  - provider_id = "llm_nvidia_nim", status = ACTIVE
  - encrypted_payload = <fernet bytes>
  - provenance = {"set_by": "operator", "source": "ui"}
  - If a prior credential existed: rotated_at = now, created_at preserved. Otherwise fresh.

  store.save(credential) in credential_store.py:19:
  - reads .keys.enc → upserts by provider_id → writes the full file back as JSON with the encrypted payload
  base64-encoded (it's a JSON file, so binary must be encoded). The raw API key never touches disk unencrypted.

  6. In-process provider reload — provider_bootstrap.reload() (application.py:257)

  This is the hot-reload. Without restarting the process, it rebuilds:
  - market snapshot service, provider registry, fundamentals providers
  - ai_advisory_provider (the LiteLLM OpenAI-compatible connection)
  - app.state.litellm_provider_environment ← this is where llm_nvidia_nim matters

  build_litellm_provider_environment() in llm_provider_secrets.py:31:
  - iterates LLM_PROVIDER_SECRET_SCHEMAS
  - for llm_nvidia_nim (LLMProviderSecretSchema("llm_nvidia_nim", "NVIDIA NIM", "NVIDIA_NIM_API_KEY")):
    - looks up the store, finds the now-active credential
    - decrypts it → gets api_key
    - adds "NVIDIA_NIM_API_KEY": "<decrypted_key>" to the environment dict

  That dict is stored in app.state.litellm_provider_environment.

  7. What litellm_provider_environment is actually used for

  This is the important nuance. That dict is used in exactly two places:

  1. Governance display only (routes.py:1954): the "Provider Secrets" section of the UI reads
  app.state.litellm_provider_environment to determine whether to show "injected" vs "configured" vs "missing" next to
  each provider. That's the injected_at_composition_boundary flag.
  2. Nowhere else in the runtime. The LiteLLM gateway is a separate Docker container (litellm service in
  docker-compose.yml:45). It reads its provider API keys from OS environment variables set at container startup:

  # docker-compose.yml:52
  NVIDIA_NIM_API_KEY: ${NVIDIA_NIM_API_KEY:-}

  The LiteLLM config (litellm_config.yaml:10) then uses:
  api_key: os.environ/NVIDIA_NIM_API_KEY

  The TradeForge runtime cannot inject that key into the already-running LiteLLM container dynamically. The LiteLLM
  container needs NVIDIA_NIM_API_KEY set in the host's shell environment before docker compose up.

  8. Response back to the frontend

  _build_status(...) decrypts the credential just to produce a masked display value (last 4 chars of the key, prefixed
  with ••••). The response carries configured: true, status: "active", and the masked field. The API key itself never
  leaves the server.

  Frontend: credential list updates in-place, the form closes, toast shows "Credential saved. Providers reloaded."

  ---
  Summary diagram

  User types key → handleSave()
    → PUT /admin/credentials/llm_nvidia_nim  { fields: { api_key: "..." } }
      → validate provider + fields
      → KeyManager.encrypt_payload()  [Fernet AES]
      → Credential(status=ACTIVE, encrypted_payload=...)
      → CredentialStore.save()  →  .keys.enc  [base64 JSON, key never plain on disk]
      → provider_bootstrap.reload()
          → build_litellm_provider_environment()

  ---
  Summary diagram

  User types key → handleSave()
    → PUT /admin/credentials/llm_nvidia_nim  { fields: { api_key: "..." } }
      → validate provider + fields
      → KeyManager.encrypt_payload()  [Fernet AES]
      → Credential(status=ACTIVE, encrypted_payload=...)
      → CredentialStore.save()  →  .keys.enc  [base64 JSON, key never plain on disk]

  User types key → handleSave()
    → PUT /admin/credentials/llm_nvidia_nim  { fields: { api_key: "..." } }
      → validate provider + fields
      → KeyManager.encrypt_payload()  [Fernet AES]
      → Credential(status=ACTIVE, encrypted_payload=...)
      → CredentialStore.save()  →  .keys.enc  [base64 JSON, key never plain on disk]
      → provider_bootstrap.reload()
          → build_litellm_provider_environment()
              → decrypts llm_nvidia_nim → { NVIDIA_NIM_API_KEY: "..." }
              → stored in app.state.litellm_provider_environment
                 (used ONLY for governance UI display — "injected" badge)
      → masked response → toast "Credential saved"

  The gap to be aware of: saving via the UI encrypts the key into TradeForge's own store and makes the
   governance page show "injected". But the separate LiteLLM Docker container only picks up
  NVIDIA_NIM_API_KEY if it was present as a host env var when docker compose up was run. Saving
  through the UI does not hot-reload the LiteLLM container.
  



The current M13B implementation achieved:

TradeForge encrypted credential governance

BUT NOT:

runtime secret propagation into LiteLLM container

That is the architectural gap.

Right now:

TradeForge UI save
    ↓
encrypted into .keys.enc
    ↓
decrypted into app.state.litellm_provider_environment
    ↓
ONLY used for governance display

NOT for actual LiteLLM runtime auth.

So your current UI badge:

NVIDIA_NIM_API_KEY
injected

is semantically misleading.

It really means:

TradeForge decrypted the key internally

NOT:

LiteLLM is currently using this key

That distinction matters enormously

The TradeForge runtime cannot inject that key into the already-running LiteLLM container dynamically.

THAT is the real architectural problem.

The TradeForge runtime cannot inject that key into the already-running LiteLLM container dynamically.

THAT is the real architectural problem.

Then TF-F074 is incomplete or incorrectly closed.

Based on the trace, it implemented encrypted storage and UI display, but **not actual LiteLLM runtime injection**.

The decisive evidence is:

```text
app.state.litellm_provider_environment
```

is used only for:

* governance display
* “injected” badge

and **not** to configure the running LiteLLM container. 

So the right next action is not a new feature issue. It is a corrective feedback issue against TF-F074:

```text
TF-F076 — Complete Managed LiteLLM Secret Injection Semantics
```


Problem statement:

```text
TF-F074 is marked Done, but encrypted LLM provider secrets are not propagated to the managed LiteLLM runtime. The UI reports provider secrets as injected even though LiteLLM still relies on container startup environment variables.
```

Acceptance:

```text
- Saving llm_nvidia_nim key causes LiteLLM to use that key, not host env.
- “Injected” means injected into LiteLLM runtime, not only TradeForge memory.
- NIM primary succeeds without falling back to Ollama.
- UI distinguishes stored/decrypted from runtime-active.
```

 create TF-F076 as a corrective issue, linked to TF-F074 and ADR-0043. as I think TF-F075 is already existing or planned and will be in M14.
 
