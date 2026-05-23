---
title: UI-Based Credential Management Planning Synthesis
type: processed-synthesis
status: processed
created: 2026-05-22
updated: 2026-05-22
source_history:
  - knowledge/raw/archived/Plan for UI-Based Credential Management.md
  - knowledge/raw/archived/20260522-tf-f055-ui-credential-management-planning.md
  - knowledge/processed/20260522-tf-f055-ui-credential-management-implementation-synthesis.md
tags: [TradeForge, credential-boundary, UI, admin-api, provider-configuration, TF-F055]
related:
  - "[[Provider Boundary Model]]"
  - "[[20260522-tf-f045-litellm-credential-shape-synthesis]]"
  - "[[20260522-tf-f055-ui-credential-management-implementation-synthesis]]"
---

# UI-Based Credential Management Planning Synthesis

## Stabilized Planning Insight

Credential management is currently operator-hostile because provider keys are
managed through `scripts/manage_credentials.py`.

The proposed next step is a restricted UI-based credential management surface in
the existing Provider Configuration experience. The plan would allow operators
to enter, rotate, mask, and revoke provider credentials from the browser while
preserving the M10C credential boundary.

## Runtime Issue

The issue is:

```text
TF-F055: Implement UI-based credential management
```

Runtime synchronization update: TF-F055 is now Done in the runtime issue
register and roadmap. Runtime ADR-0037 includes the UI credential management
amendment, and `DOCS/credential-ui-strategy.md` preserves the implementation
design.

## ADR Assessment

The plan should amend ADR-0037 rather than create a new ADR.

The encryption model, master-key model, and no-secrets-in-code rule remain
unchanged. The proposed amendment would add:

- restricted runtime access to CredentialStore through an admin credential API
- automatic in-process provider reload after credentials are saved or revoked

This is an extension of the operational credential boundary, not a replacement
for it.

## Backend Shape

The backend implementation introduced a `ProviderBootstrapService` that
rebuilds provider-dependent services in-process after credential changes.

The admin API surface:

```text
GET    /admin/credentials
PUT    /admin/credentials/{provider_id}
DELETE /admin/credentials/{provider_id}
```

Rules:

- plaintext secrets must never be returned
- masked fields may show only a short suffix
- revocation preserves the credential record for audit
- `TRADEFORGE_MASTER_KEY` remains environment-only
- missing master key returns a clear 503, not a UI setup path
- provider reload occurs after successful save or revoke

## Frontend Shape

The implementation extends `ProviderConfigurationPanel` rather than adding a new
page.

Provider rows would show inline credential sections:

- configured status with masked fields
- not-configured status with required input fields
- password inputs for secret values
- submit flow through the admin credential API
- confirmation after provider reload

The frontend uses a static provider credential schema for current providers,
including `litellm` with `base_url`, `api_key`, and `default_model`.

## Boundary Preservation

UI credential management must not weaken credential doctrine.

It must not:

- expose plaintext secrets
- store keys in frontend state longer than form submission requires
- configure `TRADEFORGE_MASTER_KEY` from the UI
- bypass `CredentialStore`
- require provider adapters to import security modules
- make provider credentials canonical business facts

Credentials remain operational capability metadata, not lifecycle state or
Event Ledger truth.

## Implementation Update

The deferred runtime actions are complete:

- ADR-0037 was amended for UI credential management.
- `DOCS/credential-ui-strategy.md` was added.
- TF-F055 was added to `DOCS/ISSUE_REGISTER.md`.
- TF-F055 was added to `DOCS/Milestone_Roadmap_v2.md`.

The implementation closeout is preserved in
[[20260522-tf-f055-ui-credential-management-implementation-synthesis]].
