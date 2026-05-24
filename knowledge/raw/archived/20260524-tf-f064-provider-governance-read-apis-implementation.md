---
title: TF-F064 Provider Governance Read APIs Implementation
type: raw-implementation
status: raw
created: 2026-05-24
source_issue: TF-F064
milestone: M13A
tags:
  - TradeForge
  - M13A
  - provider-governance
  - runtime-implementation
---

# TF-F064 Provider Governance Read APIs Implementation

## Implementation Summary

Implemented `GET /provider-governance` in the runtime API as a read-only operational provider governance read model. The endpoint composes existing provider registry state, credential-store status, capability route resolution, diagnostic class metadata, and LiteLLM AI gateway route alias metadata.

The response explicitly reports:

- `authority=operational`
- `is_canonical=false`
- `lifecycle_authority=false`
- `event_ledger_writes=false`
- provider capability metadata
- credential status without plaintext, masked, or display credential fields
- capability route primary/fallback/selected/degraded state
- diagnostics summary with no retained history and no event-ledger authority
- LiteLLM gateway route aliases for `tf-fast`, `tf-reasoning`, `tf-long-context`, `tf-cheap`, and `tf-local`

## Files Changed

- `src/app/api/routes.py`
- `tests/test_provider_governance_api.py`
- `DOCS/ISSUE_REGISTER.md`
- `DOCS/Milestone_Roadmap_v2.md`

## Event, Lifecycle, And Replay Impact

No event types were added. The endpoint does not append to the event store. It reports current operational state only and does not reconstruct historical provider or gateway health. Lifecycle authority remains false.

## Verification

Executed:

- `uv run pytest tests/test_provider_governance_api.py tests/test_fundamentals_overlay.py tests/test_admin_credentials.py tests/test_default_advisory_provider_bootstrap.py`
- `uv run mypy src\app\api\routes.py tests\test_provider_governance_api.py`
- `uv run ruff check tests\test_provider_governance_api.py`
- `git diff --check`

Focused tests passed: 24 passed. Mypy passed for the changed API/test files. The new test file passes ruff. `git diff --check` passed with existing line-ending warnings.

Full ruff over `src/app/api/routes.py` still reports pre-existing long-line and FastAPI `Query(...)` default findings outside the TF-F064 changes.

## Follow-Up

TF-F065 can add validation/test workflows and persisted validation timestamps. TF-F066 can deepen AI gateway route visibility, including route reachability and underlying model/provider metadata where available.
