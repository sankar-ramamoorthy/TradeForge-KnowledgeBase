# TF-F055: UI-Based Credential Management — Planning Note

## Date
2026-05-22

## Issue
TF-F055 — Implement UI-based credential management

## Problem
Entering and rotating provider API keys currently requires a CLI script
(`scripts/manage_credentials.py`). This is cumbersome for operator use.
The operator should be able to enter keys from ProviderConfigurationPanel
in the browser UI without touching a terminal.

## Approach

### ADR
Amend ADR-0037 (not a new ADR). Two addenda:
1. CredentialStore is accessible at runtime via restricted /admin/credentials API
2. Provider registry reloads in-process after a credential is saved — no restart

### Backend

**ProviderBootstrapService** (new class in application.py)
- Holds reference to app and credential_store
- `reload()` method rebuilds MarketSnapshotService, FundamentalsService,
  ProviderRegistry from current credential_store contents
- Attached to app.state.provider_bootstrap

**Expose app.state.credential_store**
Currently _credential_store is a local variable. Expose it on app.state so
admin routes and ProviderBootstrapService can access it consistently.

**Admin routes** (new src/app/api/admin_routes.py, mounted at /admin)
- GET /admin/credentials — list all known providers with configured status
  and masked field values (last 4 chars only). Never returns plaintext secrets.
- PUT /admin/credentials/{provider_id} — save/update credential, then reload.
  Returns same shape as a single GET item.
- DELETE /admin/credentials/{provider_id} — revoke (set status=REVOKED).
  Calls reload().

Known providers (static registry): yfinance (no creds needed), polygon,
alpaca, fmp, alpha_vantage, litellm.

MasterKeyNotConfiguredError → 503 with explanatory detail.
If credential_store doesn't exist yet, create it on first PUT.

### Frontend

**PROVIDER_CREDENTIAL_SCHEMAS** (static const in runtime.ts)
Maps provider_id to array of {name, label, secret} field descriptors.
Static — no API round-trip needed. Frontend knows field schema at build time.

**New API functions** (runtime.ts)
- fetchCredentials() → GET /admin/credentials
- updateCredential(provider_id, fields) → PUT /admin/credentials/{provider_id}
- revokeCredential(provider_id) → DELETE /admin/credentials/{provider_id}

**ProviderConfigurationPanel expansion**
Add a "Credential Management" collapsible section beneath existing provider
display. For each provider (polygon, alpaca, fmp, alpha_vantage, litellm):
- Configured (green badge) or Not configured (grey badge)
- Masked field display when configured (e.g., api_key: ••••ab12)
- "Update" button → toggles inline form with password inputs
- Submit → PUT → success toast "Providers reloaded"
- No modal, no new page — inline expansion of the panel

yfinance is shown as "no credentials required" — always functional.

## Files Modified/Created
- src/app/api/application.py — ProviderBootstrapService, expose credential_store
- src/app/api/admin_routes.py (NEW) — admin credential endpoints
- frontend/src/api/runtime.ts — PROVIDER_CREDENTIAL_SCHEMAS + 3 new functions
- frontend/src/workspaces/ProviderConfigurationPanel.tsx — credential section
- DOCS/adr/0037-operational-credential-boundary.md — amendment section
- DOCS/credential-ui-strategy.md (NEW) — design document
- DOCS/ISSUE_REGISTER.md — TF-F055 entry
- DOCS/Milestone_Roadmap_v2.md — TF-F055 in feedback issues

## Security Notes
- TRADEFORGE_MASTER_KEY stays in environment — not configurable via UI
- Secrets never returned from GET (last 4 chars of masked value only)
- Credential store path is always .keys.enc at project root
- PUT/DELETE/GET all require master key to be set (503 otherwise)
- No secrets logged, no secrets in response bodies

## Invariants Preserved
- ADR-0037: CredentialStore remains sole write path for credentials
- create_app() composition root pattern unchanged for startup
- Provider adapters don't know about credential storage
