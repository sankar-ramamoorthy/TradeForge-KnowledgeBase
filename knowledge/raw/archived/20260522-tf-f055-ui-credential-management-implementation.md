# TF-F055: UI-Based Credential Management — Implementation Note

## Date
2026-05-22

## Status
Done. All tests pass. Branch: feature/tf-f055-ui-credential-management

## What Was Built

### Backend

**`ProviderBootstrapService`** (new class in `src/app/api/application.py`)
Rebuilds provider-dependent services in-place after a credential change:
- `market_snapshot_service` (with fresh provenance/snapshot stores)
- `contextual_summary_service`
- `provenance_query_service`
- `market_snapshot_query_service`
- `provider_registry`
- `fundamentals_service`

Attached to `app.state.provider_bootstrap`. The admin route calls `reload()`
after every PUT/DELETE. Handles `MasterKeyNotConfiguredError` gracefully (returns
without rebuilding) so startup failures don't cascade as runtime failures.

**`src/app/api/admin_routes.py`** (new file)
`admin_router` mounted at `/admin`. Three endpoints:
- `GET /admin/credentials` — lists all six known providers with configured status
  and masked field values. Works even without master key (shows degraded state).
- `PUT /admin/credentials/{provider_id}` — encrypts + saves credential, calls reload.
  Returns 503 if master key not set, 422 if provider unknown or fields missing.
- `DELETE /admin/credentials/{provider_id}` — sets status=REVOKED, calls reload.

**Fix to `_default_fundamentals_providers()`**
Previously called `KeyManager.from_environment()` unconditionally when any
`CredentialStore` was present. Now only calls it when FMP or Alpha Vantage
credentials actually exist in the store. This prevents `MasterKeyNotConfiguredError`
in test environments with empty stores.

**Exposed `app.state.credential_store`**
Previously `_credential_store` was a local variable in `create_app()`. Now exposed
on `app.state` so admin routes can access and (if needed) create it on first PUT.

### Frontend

**`PROVIDER_CREDENTIAL_SCHEMAS`** in `frontend/src/api/runtime.ts`
Static mapping of provider_id → field descriptors. Each field has `name`, `label`,
`secret` (bool). Secret fields rendered as `type="password"`. Non-secret fields
(litellm `base_url`, `default_model`) rendered as plain text inputs.

**Three new API functions** in `frontend/src/api/runtime.ts`:
- `fetchCredentials()` → `GET /admin/credentials`
- `updateCredential(provider_id, fields)` → `PUT /admin/credentials/{provider_id}`
- `revokeCredential(provider_id)` → `DELETE /admin/credentials/{provider_id}`

**`ProviderConfigurationPanel.tsx`** expansion:
- Loads credential list alongside provider configuration on mount
- New "Credentials" section with per-provider rows
- Configured (green) / Not configured (grey) badge per provider
- Masked field display when configured (e.g., `api_key: ••••ab12`)
- "Update" button → toggles inline form with password inputs
- "Revoke" button (when configured) → DELETE with confirmation
- Form submission → PUT → "Credential saved. Providers reloaded." toast
- Master key warning if `master_key_configured: false`
- yfinance shows "no credentials required" always

### Tests
13 new tests in `tests/test_admin_credentials.py`:
- List returns all known providers including yfinance
- Unconfigured providers show not-configured status
- Masked display correct (last 4 chars of secret, full text for non-secret)
- PUT saves, encrypts, triggers reload
- PUT rejects unknown provider (422), missing fields (422), no master key (503)
- DELETE sets status=revoked, preserves record
- DELETE 404 if not configured
- LiteLLM non-secret fields show display_value (base_url, model)
- Second PUT sets rotated_at correctly

Important: `_master_key` context manager RESTORES the original env var on exit
(doesn't blindly remove it). `_no_master_key` context manager TEMPORARILY UNSETS
it and restores on exit. Both patterns are needed to avoid polluting other tests
that depend on the system `TRADEFORGE_MASTER_KEY`.

## Architectural Notes

ADR-0037 amendment documents the two new decisions:
1. CredentialStore accessible at runtime via /admin/credentials
2. In-process provider reload after credential change

The original invariants are preserved:
- Master key stays in OS environment only
- Provider adapters unaware of credential storage
- create_app() remains the startup composition root
- No secrets in response bodies or application code

## What This Enables

After this issue, the operator configures ALL provider credentials from
the browser UI. The only terminal interaction needed is:
1. Generate TRADEFORGE_MASTER_KEY once (`manage_credentials.py generate-master-key`)
2. Set it in the OS environment

Everything else — polygon key, FMP key, LiteLLM endpoint — is manageable
from ProviderConfigurationPanel without a terminal.
