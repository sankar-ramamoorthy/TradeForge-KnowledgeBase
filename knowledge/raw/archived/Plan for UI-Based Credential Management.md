 
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌
 Plan: UI-Based Credential Management

 Context

 Managing provider API keys currently requires a CLI script
 (scripts/manage_credentials.py). The user wants to enter and rotate keys
 from the existing ProviderConfigurationPanel in the browser UI. This
 extends the M10C credential boundary (ADR-0037) to allow runtime write access
 to CredentialStore via a restricted admin API surface.

 ---
 ADR Decision

 Amend ADR-0037 rather than create a new ADR. The encryption model, master
 key model, and no-secrets-in-code rule are unchanged. Two new decisions are
 addenda to the same boundary:
 1. CredentialStore is accessible at runtime via a restricted /admin/credentials
 API surface (not only at startup).
 2. After a credential is saved, an in-process provider registry reload happens
 automatically — no restart required.

 New file: DOCS/adr/0037-operational-credential-boundary.md gets an
 ## Amendment — UI Credential Management (TF-F055) section appended.

 A new DOCS/credential-ui-strategy.md document covers the full design
 (mirroring the pattern of DOCS/llm-adapter-strategy.md).

 ---
 New Issue

 TF-F055 — Implement UI-based credential management
 Milestone: M13 (or feedback series)
 Linked ADRs: ADR-0037 amendment
 Branch: feature/tf-f055-ui-credential-management

 ---
 Implementation Plan

 1. Backend — ProviderBootstrapService (src/app/api/application.py)

 Extract the provider-instantiation logic currently inline in create_app()
 into a ProviderBootstrapService class attached to app.state. Expose a
 reload() method that re-runs the same bootstrap (decrypt credentials,
 instantiate adapters, register in provider registry).

 Key files:
 - src/app/api/application.py — extract bootstrap logic, attach to app.state

 2. Backend — Admin Credential Routes (src/app/api/routes.py or new admin_routes.py)

 New router mounted at /admin/credentials:

 GET  /admin/credentials
      Returns: list of { provider_id, configured, status, rotated_at,
                          masked_fields: { api_key: "sk-••••ab12" } }
      Secrets never returned. Last-4 chars only in masked_fields.

 PUT  /admin/credentials/{provider_id}
      Body: { fields: { api_key: "...", secret_key: "..." } }
      Encrypts via KeyManager, writes via CredentialStore,
      calls app.state.provider_bootstrap.reload().
      Returns same single-provider shape as one GET item.

 DELETE /admin/credentials/{provider_id}
      Sets status=REVOKED. Record preserved for audit trail.
      Calls reload().

 Master key unavailability (MasterKeyNotConfiguredError) returns 503 with
 a clear message — it cannot be configured via UI, only via environment.

 Key files:
 - src/app/api/routes.py (or new src/app/api/admin_routes.py)
 - src/app/api/application.py — wire new router

 3. Frontend — Provider Schema Registry (frontend/src/api/runtime.ts)

 Add static PROVIDER_CREDENTIAL_SCHEMAS constant:

 export const PROVIDER_CREDENTIAL_SCHEMAS = {
   polygon:       [{ name: "api_key",       label: "API Key",       secret: true }],
   alpaca:        [{ name: "api_key",       label: "API Key",       secret: true },
                   { name: "secret_key",    label: "Secret Key",    secret: true }],
   fmp:           [{ name: "api_key",       label: "API Key",       secret: true }],
   alpha_vantage: [{ name: "api_key",       label: "API Key",       secret: true }],
   yfinance:      [],
   litellm:       [{ name: "base_url",      label: "Base URL",      secret: false },
                   { name: "api_key",       label: "API Key",       secret: true },
                   { name: "default_model", label: "Default Model", secret: false }],
 } as const;

 Key files:
 - frontend/src/api/runtime.ts

 4. Frontend — ProviderConfigurationPanel expansion

 Expand each provider row in-place (no modal, no new page). Add a collapsible
 credential subsection beneath each provider entry:

 - Configured (green dot): shows masked fields + "Update" inline toggle
 - Not configured (grey dot): shows inline form with required fields
 - All secret fields use type="password" inputs
 - On submit: PUT fires → success shows masked confirmation + rotated_at
 - On reload: transient "Providers reloaded" toast

 Key files:
 - frontend/src/workspaces/ProviderConfigurationPanel.tsx

 ---
 Documents to Write

 1. DOCS/adr/0037-operational-credential-boundary.md — append amendment section
 2. DOCS/credential-ui-strategy.md — full design doc (background, security
 tradeoffs, API contract, reload model, frontend design, what does NOT change)
 3. DOCS/ISSUE_REGISTER.md — add TF-F055 to index and full spec
 4. DOCS/Milestone_Roadmap_v2.md — add TF-F055 to M13 linked issues

 ---
 Verification

 - uv run pytest — full suite passes
 - uv run mypy src tests — no type errors
 - uv run ruff check . — no lint errors
 - npm.cmd run typecheck && npm.cmd run build — frontend compiles
 - Manual: start app, open ProviderConfigurationPanel, enter a Polygon key,
 confirm masked display, confirm market data loads without restart
 - Manual: update a key, confirm "Providers reloaded" toast, confirm new key
 is used immediately
 - Manual: attempt GET /admin/credentials — confirm no plaintext secrets in
 response body