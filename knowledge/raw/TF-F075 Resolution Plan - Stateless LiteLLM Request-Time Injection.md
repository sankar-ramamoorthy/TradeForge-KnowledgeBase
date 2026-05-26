
• Proposed Plan


  # TF-F075 Resolution Plan: Stateless LiteLLM Request-Time Injection

  ## Summary

  Reopen TF-F075 and correct the architecture from DB-backed LiteLLM config mutation to stateless request-time
  credential composition.

  Problem statement to record in the issue: the /config/update approach made TradeForge depend on LiteLLM DB-backed
  runtime config, exposed schema/version drift, and contradicted the intended boundary where TradeForge owns
  credentials and LiteLLM is only an internal adapter.

  Target resolution:

  TradeForge CredentialStore
    -> resolve selected advisory model
    -> infer required provider secret from model/provider prefix
    -> decrypt provider key per request
    -> POST internal LiteLLM /chat/completions with model/api_key/api_base

  ## Key Changes

  - Reopen TF-F075 in DOCS/ISSUE_REGISTER.md.
      - Change status from Done to In Progress or Reopened.
      - Preserve the DB-backed /config/update attempt as rejected implementation history.
      - Add the 2026-05-26 KB synthesis as source context.
      - Make acceptance criteria require no LiteLLM DB and no /config/update.
  - Amend ADR-0043.
      - Replace the accepted /config/update implementation note with request-time composition.
      - Mark LiteLLM DB-backed config mutation as rejected for M13B.
      - Keep the accepted governance boundary: TradeForge owns downstream LLM provider secrets.
  - Runtime correction.
      - Remove LiteLLMGatewayAdminClient and all startup/reload calls that push secrets to LiteLLM.
      - Remove LiteLLM DATABASE_URL, STORE_MODEL_IN_DB=True, litellm_proxy init SQL, and provider-secret environment
        variables from normal Compose.
      - Keep LiteLLM internal-only via expose: "4000".
      - Pin the LiteLLM image to an explicit stable tag instead of main-latest; verify the tag at implementation time
        with the registry before editing.
  - Advisory provider behavior.
      - Replace OpenAI SDK calls for advisory generation with direct internal HTTP calls to LiteLLM /chat/completions,
        authenticated with LITELLM_MASTER_KEY.
      - Build the request body dynamically with model, messages, temperature, max_tokens, and, when needed, api_key /
        api_base.
      - Infer provider credential from model prefix, initially supporting groq/, openai/, anthropic/, gemini/,
        google/, nvidia_nim/, and keyless ollama/.
      - Preserve existing primary/fallback model behavior, but allow fully dynamic model strings instead of requiring
        LiteLLM model discovery.
  - Governance API/UI.
      - Rename injected_at_composition_boundary to request-time readiness wording, e.g.
        available_for_runtime_injection.
      - Update Provider Governance UI labels from injected to ready, configured, or missing.
      - Keep responses secret-safe: no plaintext key, no request body logging, no leaked suffixes.

  ## Test Plan

  - Update provider governance API tests:
      - configured provider secret reports available_for_runtime_injection=True when its credential is active
      - no LiteLLM credential or inactive provider secret reports not ready
      - response never contains plaintext secret values
  - Replace test_litellm_admin_client.py with request-composition tests:
      - Groq model adds decrypted api_key
      - Ollama model omits api_key and includes configured api_base
      - fallback model gets its own provider resolution
      - missing required provider key raises advisory provider unavailable
      - logs never include secret payloads
  - Update Compose/config tests:
      - no provider API keys in LiteLLM env
      - LiteLLM image is not main-latest
      - scripts/postgres-init/02-litellm-db.sql no longer exists
  - Run verification:
      - uv run pytest tests/test_openai_compatible_provider.py tests/test_provider_governance_api.py tests/
        test_postgres_compose.py tests/test_llm_provider_secrets.py
      - relevant mypy and ruff
      - docker compose --profile advisory config
      - frontend typecheck/build if UI contract changes touch TypeScript

  ## Assumptions

  - TF-F075 is reopened rather than replaced by TF-F076.
  - Model selection becomes fully dynamic; LiteLLM model_list is not the source of truth.
  - TradeForge remains the only credential authority.
  - LiteLLM remains internal advisory infrastructure and does not store provider secrets.
  - No event taxonomy or lifecycle behavior changes are required.

  Reference context: LiteLLM docs describe the proxy as an OpenAI-compatible gateway and DB-backed features as
  separate gateway management capabilities: https://docs.litellm.ai/ and https://docs.litellm.ai/docs/proxy/db_info.
