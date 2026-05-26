---
title: TF-F074 Governed LLM Provider Secret Management Implementation Note
type: implementation-note
status: raw
created: 2026-05-25
issue: TF-F074
milestone: M13B
tags:
  - tf-f074
  - m13b
  - llm-provider-secrets
  - credential-governance
  - litellm
---

# TF-F074 Governed LLM Provider Secret Management Implementation Note

## Context

TF-F074 completes the M13B managed advisory runtime slice by moving downstream
LLM provider key governance into TradeForge's encrypted credential boundary.

## Implementation Summary

Added governed downstream LLM provider credential schemas:

- `llm_groq`
- `llm_nvidia_nim`
- `llm_openai`
- `llm_anthropic`
- `llm_google`

These credentials use the existing encrypted `.keys.enc` store and existing
admin credential API/CLI patterns. UI/API responses show masked values only.

Added `src/security/llm_provider_secrets.py`, which maps governed provider
credentials to LiteLLM environment variable names:

- `GROQ_API_KEY`
- `NVIDIA_NIM_API_KEY`
- `OPENAI_API_KEY`
- `ANTHROPIC_API_KEY`
- `GOOGLE_API_KEY`

The environment projection is built only at the runtime composition boundary
and refreshed on provider reload.

Provider Governance now exposes:

```text
GET /provider-governance/ai-gateway/provider-secret-injection
```

The endpoint returns configured/injected status and target environment variable
names without returning secret values.

## Boundary Notes

No lifecycle behavior changed.

No event types were added.

No AI authority changed.

No direct vendor SDK bypass was added.

The LiteLLM static config references provider environment variable names only,
not plaintext provider keys.

## Verification

- `uv run pytest tests/test_llm_provider_secrets.py tests/test_admin_credentials.py tests/test_provider_governance_api.py tests/test_postgres_compose.py tests/test_manage_credentials_script.py tests/test_credential_store.py`
- `uv run mypy src\security src\app\api\application.py src\app\api\admin_routes.py src\app\api\routes.py tests\test_llm_provider_secrets.py tests\test_admin_credentials.py tests\test_provider_governance_api.py`
- `uv run ruff check src\security src\app\api\application.py src\app\api\admin_routes.py tests\test_llm_provider_secrets.py tests\test_admin_credentials.py tests\test_provider_governance_api.py tests\test_manage_credentials_script.py tests\test_postgres_compose.py scripts\manage_credentials.py`
- `npm.cmd run typecheck`
- `npm.cmd run build`
- `docker compose --profile advisory config`
