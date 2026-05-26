---
title: TF-F065 Credential Validation Implementation
type: raw-implementation
status: raw
created: 2026-05-24
source_issue: TF-F065
milestone: M13A
tags:
  - TradeForge
  - M13A
  - credential-validation
  - runtime-implementation
---

# TF-F065 Credential Validation Implementation

## Implementation Summary

Implemented explicit credential validation through `POST /admin/credentials/{provider_id}/validate`.

Validation behavior:

- rejects unknown providers
- rejects providers that do not require credentials
- returns 404 for missing credentials
- returns 409 for revoked credentials
- decrypts the saved credential with the OS-provided `TRADEFORGE_MASTER_KEY`
- verifies required credential fields are present and non-empty
- persists `last_validated_at` and `active` status on success
- persists `invalid` status on unreadable or malformed payloads
- never returns plaintext secrets

Additional changes:

- Added `invalid` to `CredentialStatus`.
- Added `last_validated_at` to admin credential status responses and frontend types.
- Added `validateCredential()` frontend API helper.
- Updated provider governance status mapping to surface invalid credentials.

## Files Changed

- `src/security/credential.py`
- `src/app/api/admin_routes.py`
- `src/app/api/routes.py`
- `frontend/src/api/runtime.ts`
- `tests/test_admin_credentials.py`
- `DOCS/ISSUE_REGISTER.md`
- `DOCS/Milestone_Roadmap_v2.md`

## Event, Lifecycle, And Replay Impact

No event types were added. Validation writes only credential-store metadata, not canonical event-ledger truth. Lifecycle authority is unchanged. Replay should not use live validation to reconstruct historical credential state.

## Verification

Executed:

- `uv run pytest tests/test_admin_credentials.py tests/test_provider_governance_api.py tests/test_default_advisory_provider_bootstrap.py`
- `uv run mypy src\app\api\admin_routes.py src\app\api\routes.py tests\test_admin_credentials.py tests\test_provider_governance_api.py`
- `uv run ruff check src\app\api\admin_routes.py tests\test_admin_credentials.py tests\test_provider_governance_api.py`
- `npm.cmd run typecheck`
- `git diff --check`

All targeted verification passed. `git diff --check` passed with line-ending warnings.

## Follow-Up

Deeper live-provider validation, route reachability checks, latency diagnostics, quota diagnostics, and retained diagnostic history remain separate M13A diagnostic or AI gateway visibility concerns.
