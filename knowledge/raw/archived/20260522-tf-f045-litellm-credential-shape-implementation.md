# TF-F045: LiteLLM Credential Shape — Implementation Note

## Date
2026-05-22

## Issue
TF-F045 — Add LiteLLM credential shape to CredentialStore

## Status
Done (cherry-picked onto M13 branch from local M12 branch commits)

## What Was Built

### New file: `src/security/litellm_credential.py`

`LiteLLMCredentialPayload` — frozen dataclass with `base_url`, `api_key`,
`default_model`. Validates non-empty fields.

`create_litellm_credential()` — creates an encrypted `Credential` record for
the `litellm` provider via `KeyManager.encrypt_payload()`.

`get_litellm_credential()` — retrieves and decrypts the LiteLLM credential
from `CredentialStore`. Raises `LiteLLMCredentialNotConfiguredError` if absent.
Validates all three required fields are present in the decrypted payload.

`LITELLM_PROVIDER_ID = "litellm"` — the canonical provider_id string.

### Updated: `src/security/__init__.py`
Exports `LiteLLMCredentialPayload`, `LiteLLMCredentialNotConfiguredError`,
`create_litellm_credential`, `get_litellm_credential`, `LITELLM_PROVIDER_ID`.

### Updated: `scripts/manage_credentials.py`
Added `litellm` to the provider registry with fields:
`base_url` (non-secret), `api_key` (secret), `default_model` (non-secret).
CLI can now register/rotate LiteLLM credentials alongside market data providers.

### Tests
`tests/test_credential_store.py` — added LiteLLM credential creation,
retrieval, and missing-credential error path.
`tests/test_manage_credentials_script.py` — added CLI registration test.

## Architectural Notes

The pattern is identical to existing market data credentials (Polygon, FMP
etc.). `KeyManager` encrypts the full payload dict; `get_litellm_credential()`
decrypts and maps to `LiteLLMCredentialPayload`. The credential boundary
(ADR-0037) is unchanged — no provider key appears outside `CredentialStore`.

`LiteLLMCredentialNotConfiguredError` is a `RuntimeError` subclass. The
composition root (`create_app()`) is the expected caller of
`get_litellm_credential()` when wiring `OpenAICompatibleAdvisoryProvider`.

## Invariants Preserved
- No key material in code, .env, or git history
- CredentialStore as sole encrypted write path
- Provider adapters remain unaware of encryption internals

## Next
TF-F046 — `OpenAICompatibleAdvisoryProvider` calls `get_litellm_credential()`
at construction time (from composition root) and issues OpenAI chat completions
calls via the `base_url` endpoint.
