---
title: TF-F074 LiteLLM Secret Injection Gap
type: brainstorm
status: raw
tags: [TradeForge, brainstorm, TF-F074, LiteLLM, credential-governance, provider-secrets]
created: 2026-05-25
---

# TF-F074 LiteLLM Secret Injection Gap

## Trigger

Feedback on `TF-F074` after tracing the end-to-end behavior for saving an `llm_nvidia_nim` credential through the TradeForge UI.

The concern is that `TF-F074` is marked complete, but the implemented behavior may only complete encrypted credential governance inside TradeForge, not actual runtime secret propagation into the managed LiteLLM container.

## Raw Input

We have a problem: `TF-F074` is complete / Done but did not actually complete correctly.

Here is the full end-to-end trace of what happens when a user enters a key for `llm_nvidia_nim` and hits Save.

### What happens when you save `llm_nvidia_nim` credentials

1. Frontend - form schema and submit

`PROVIDER_CREDENTIAL_SCHEMAS["llm_nvidia_nim"]` in `runtime.ts:526` defines one field:

```text
{ name: "api_key", label: "API Key", secret: true }
```

The field renders as `<input type="password">`. On submit, `handleSave("llm_nvidia_nim")` in `ProviderConfigurationPanel.tsx:69` runs. It:

- reads `PROVIDER_CREDENTIAL_SCHEMAS` for `llm_nvidia_nim` (just `api_key`)
- strips empty optional fields (none here, because `api_key` is required)
- calls `updateCredential("llm_nvidia_nim", { api_key: "<typed_value>" })`

2. Frontend to backend - HTTP call

`updateCredential` in `runtime.ts:569` sends:

```text
PUT /admin/credentials/llm_nvidia_nim
Content-Type: application/json
{ "fields": { "api_key": "<typed_value>" } }
```

3. Backend validation

`update_credential` in `admin_routes.py:218`:

- confirms `llm_nvidia_nim` is in `_PROVIDER_FIELD_NAMES`
- confirms `api_key` is present in the payload
- sets `credential_type` to `api_key`

4. Encryption

`KeyManager.from_environment()` reads `TRADEFORGE_MASTER_KEY` from the OS environment and creates a Fernet instance.

`encrypt_payload({"api_key": "<value>"})` normalizes the payload, JSON-serializes it, Fernet-encrypts it, and returns opaque bytes.

5. Credential object is built and stored in `.keys.enc`

A `Credential` dataclass is constructed with:

- `provider_id = "llm_nvidia_nim"`
- `status = ACTIVE`
- `encrypted_payload = <fernet bytes>`
- `provenance = {"set_by": "operator", "source": "ui"}`
- if a prior credential existed, `rotated_at = now` and `created_at` is preserved

`store.save(credential)` in `credential_store.py:19`:

- reads `.keys.enc`
- upserts by `provider_id`
- writes the full file back as JSON with the encrypted payload base64-encoded
- the raw API key never touches disk unencrypted

6. In-process provider reload

`provider_bootstrap.reload()` in `application.py:257` hot-reloads in-process TradeForge provider composition.

It rebuilds:

- market snapshot service
- provider registry
- fundamentals providers
- `ai_advisory_provider`
- `app.state.litellm_provider_environment`

`build_litellm_provider_environment()` in `llm_provider_secrets.py:31`:

- iterates `LLM_PROVIDER_SECRET_SCHEMAS`
- for `llm_nvidia_nim`, maps to `NVIDIA_NIM_API_KEY`
- looks up the active credential
- decrypts it
- adds `NVIDIA_NIM_API_KEY: "<decrypted_key>"` to the environment dict

That dict is stored in `app.state.litellm_provider_environment`.

7. What `litellm_provider_environment` is actually used for

This is the important nuance. That dict is used in exactly two places:

- Governance display only, where the Provider Secrets section reads `app.state.litellm_provider_environment` to determine whether to show `injected`, `configured`, or `missing`.
- Nowhere else in the runtime.

The LiteLLM gateway is a separate Docker container. It reads provider API keys from OS environment variables set at container startup:

```yaml
NVIDIA_NIM_API_KEY: ${NVIDIA_NIM_API_KEY:-}
```

The LiteLLM config then uses:

```yaml
api_key: os.environ/NVIDIA_NIM_API_KEY
```

The TradeForge runtime cannot inject that key into the already-running LiteLLM container dynamically. The LiteLLM container needs `NVIDIA_NIM_API_KEY` set in the host shell environment before `docker compose up`.

8. Response back to the frontend

`_build_status(...)` decrypts the credential just to produce a masked display value. The response carries `configured: true`, `status: "active"`, and the masked field. The API key itself never leaves the server.

The frontend credential list updates in-place, the form closes, and the toast shows `Credential saved. Providers reloaded.`

### Summary diagram

```text
User types key -> handleSave()
  -> PUT /admin/credentials/llm_nvidia_nim { fields: { api_key: "..." } }
    -> validate provider + fields
    -> KeyManager.encrypt_payload()
    -> Credential(status=ACTIVE, encrypted_payload=...)
    -> CredentialStore.save() -> .keys.enc
    -> provider_bootstrap.reload()
      -> build_litellm_provider_environment()
        -> decrypts llm_nvidia_nim -> { NVIDIA_NIM_API_KEY: "..." }
        -> stored in app.state.litellm_provider_environment
           (used only for governance UI display - "injected" badge)
    -> masked response -> toast "Credential saved"
```

The gap to be aware of: saving via the UI encrypts the key into TradeForge's own store and makes the governance page show `injected`. But the separate LiteLLM Docker container only picks up `NVIDIA_NIM_API_KEY` if it was present as a host env var when Docker Compose started.

Saving through the UI does not hot-reload the LiteLLM container.

The current M13B implementation achieved:

```text
TradeForge encrypted credential governance
```

But not:

```text
runtime secret propagation into LiteLLM container
```

That is the architectural gap.

Right now:

```text
TradeForge UI save
  -> encrypted into .keys.enc
  -> decrypted into app.state.litellm_provider_environment
  -> only used for governance display
```

It is not used for actual LiteLLM runtime auth.

So the current UI badge:

```text
NVIDIA_NIM_API_KEY
injected
```

is semantically misleading. It really means:

```text
TradeForge decrypted the key internally
```

Not:

```text
LiteLLM is currently using this key
```

That distinction matters enormously.

The TradeForge runtime cannot inject that key into the already-running LiteLLM container dynamically. That is the real architectural problem.

Then `TF-F074` is incomplete or incorrectly closed.

Based on the trace, it implemented encrypted storage and UI display, but not actual LiteLLM runtime injection.

The decisive evidence is:

```text
app.state.litellm_provider_environment
```

is used only for:

- governance display
- `injected` badge

and not to configure the running LiteLLM container.

So the right next action is not a new feature issue. It is a corrective feedback issue against `TF-F074`:

```text
TF-F076 - Complete Managed LiteLLM Secret Injection Semantics
```

Problem statement:

```text
TF-F074 is marked Done, but encrypted LLM provider secrets are not propagated to the managed LiteLLM runtime. The UI reports provider secrets as injected even though LiteLLM still relies on container startup environment variables.
```

Acceptance:

```text
- Saving llm_nvidia_nim key causes LiteLLM to use that key, not host env.
- "Injected" means injected into LiteLLM runtime, not only TradeForge memory.
- NIM primary succeeds without falling back to Ollama.
- UI distinguishes stored/decrypted from runtime-active.
```

Create `TF-F076` as a corrective issue, linked to `TF-F074` and `ADR-0043`. `TF-F075` may already exist or be planned for M14.

## Observations

- `TF-F074` appears to complete encrypted credential capture, validation, encrypted storage, masking, and internal TradeForge reload.
- The saved secret is decrypted into `app.state.litellm_provider_environment`.
- The traced usage indicates `app.state.litellm_provider_environment` only affects governance display, not the live LiteLLM container.
- LiteLLM still reads `NVIDIA_NIM_API_KEY` from container startup environment.
- The UI label `injected` may overstate the runtime effect of the saved credential.

## Ideas

- Treat this as corrective feedback against `TF-F074`, not as unrelated new feature work.
- Create `TF-F076 - Complete Managed LiteLLM Secret Injection Semantics`.
- Separate credential states in UI language:
  - stored
  - decrypted by TradeForge
  - active in LiteLLM runtime
- Revisit whether managed LiteLLM should support runtime secret reload, container restart/recomposition, generated config materialization, or another explicit composition boundary.
- Link the corrective issue to `TF-F074` and `ADR-0043` so the runtime-secret semantics stay connected to the original architecture decision.

## Questions

- Is `TF-F074` considered incomplete, or should it be re-scoped as encrypted credential governance only?
- What is the intended mechanism for propagating governed secrets into the LiteLLM runtime?
- Should the UI ever use the word `injected` unless the managed LiteLLM process is actually using the secret?
- Does `TF-F075` already cover adjacent M14 work, requiring this corrective issue to become `TF-F076`?
- Does `ADR-0043` specify enough about the distinction between TradeForge-held provider secrets and LiteLLM-runtime-active secrets?

## Concerns

- Current status language may create false operator confidence that NIM auth is active.
- Governance display could imply runtime readiness while the real LiteLLM container remains unauthenticated.
- NIM primary may continue to fail and fall back to Ollama even after the UI reports successful credential save.
- Treating this as complete may leave a semantic gap between managed gateway governance and actual provider authentication.
- Any fix must preserve the AI advisory boundary: TradeForge may govern provider configuration, but that does not make AI outputs authoritative.

## Possible Next Outputs

- Issue candidate: `TF-F076 - Complete Managed LiteLLM Secret Injection Semantics`
- ADR follow-up or ADR-0043 clarification
- UI copy/state model update for provider secret status
- Runtime architecture note on managed LiteLLM secret propagation boundary
- No canonical promotion yet
