---
title: TF-0049 Implementation — Contextual Operational Summaries
type: implementation-capture
status: raw
issue: TF-0049
milestone: M9
created: 2026-05-13
---

# TF-0049 Implementation: Contextual Operational Summaries

## Summary

Implemented `ContextualSummaryService` combining workspace operational summaries
(TF-0023) with advisory market context (TF-0047/0048). Endpoint
`GET /workspaces/contextual-summary` returns combined view. `ContextualBriefingPanel`
integrated into OperatingWorkspace. 17 tests, 376 total passing.

## Files Changed

| File | Action |
|---|---|
| `src/services/market/contextual_summary.py` | New — `MarketContextNote`, `ContextualOperationalSummary`, `ContextualSummaryService` |
| `src/app/api/routes.py` | Added `ContextualMarketNoteResponse`, `ContextualSummaryResponse`, `_contextual_summary_service_from`, endpoint |
| `src/app/api/application.py` | Extracted `_market_svc` to reuse across snapshot + contextual summary services; wired `ContextualSummaryService` |
| `frontend/src/api/runtime.ts` | Added `ContextualMarketNote`, `ContextualSummary` types + `fetchContextualSummary` |
| `frontend/src/workspaces/ContextualBriefingPanel.tsx` | New — regime-aware market notes + operational headline in one panel |
| `frontend/src/workspaces/OperatingWorkspace.tsx` | Added `<ContextualBriefingPanel />` |
| `frontend/src/styles.css` | Added contextual briefing panel CSS |
| `tests/test_contextual_summary.py` | 17 tests |

## Key Implementation Notes

### Service Composition
`ContextualSummaryService` wraps `WorkspaceSummaryReadService` + optional
`MarketSnapshotService`. Market fetch failures are caught silently in
`_fetch_market_notes()` — market unavailability never loses the workspace summary.

### Market Service Reuse
`create_app()` now extracts `_market_svc` to a local variable before assigning
to `app.state`, enabling reuse for both `market_snapshot_service` and
`ContextualSummaryService` without double instantiation.

### Endpoint Ordering
`GET /workspaces/contextual-summary` is registered before `/market-context` and
`/{route_id}` to prevent dynamic segment capture.

### Authority Labels
- Workspace operational state: "derived"
- Market context notes: "advisory" (is_advisory=True on each note)
- Combined summary: "derived" overall (the most conservative label)

### `ContextualSummaryService` injectable
`create_app()` accepts optional `contextual_summary_service` param for test
injection, following the established pattern for all services.

## Tests Executed

- `uv run pytest tests/test_contextual_summary.py` — 17 passed
- `uv run pytest` — 376 passed
- `uv run ruff check .` — clean
- `uv run mypy src tests` — clean (86 files)
- `npm.cmd run typecheck` — clean
- `npm.cmd run build` — clean (242.95 kB JS bundle)

## M9 Workspace Context: Status

| Issue | Status |
|---|---|
| TF-0047: Market context workspace overlays | Done |
| TF-0048: Market regime interpretation model | Done |
| TF-0049: Contextual operational summaries | Done |
| TF-0050: Provider provenance tracking | Planned |

## Follow-up Opportunities

- TF-0050: Provider provenance tracking registry (formal tracking of which providers supplied context)
- Future: extend contextual summary to all workspaces, not just operating
- Future: extract ticker from lifecycle event payload so panel pre-populates
