---
title: TF-0047 Implementation — Market Context Workspace Overlays
type: implementation-capture
status: raw
issue: TF-0047
milestone: M9
created: 2026-05-13
---

# TF-0047 Implementation: Market Context Workspace Overlays

## Summary

Implemented `GET /workspaces/market-context` endpoint backed by
`MarketSnapshotService(YFinanceProvider())`. Frontend `MarketContextPanel` component
added to Opportunity and Active Position workspaces. All ADVISORY authority signals
explicit throughout.

## Files Changed

| File | Action |
|---|---|
| `src/app/api/routes.py` | Added `MarketSnapshotOverlayResponse`, `MarketContextOverlayResponse` models, `_market_snapshot_service_from` extractor, `GET /workspaces/market-context` endpoint |
| `src/app/api/application.py` | Added `market_snapshot_service` param + `MarketSnapshotService(YFinanceProvider())` default wiring |
| `frontend/src/api/runtime.ts` | Added `MarketSnapshotOverlay`, `MarketContextOverlay` types + `fetchMarketContext` function |
| `frontend/src/workspaces/MarketContextPanel.tsx` | New advisory overlay component with symbol input + OHLCV display |
| `frontend/src/workspaces/OpportunityWorkspace.tsx` | Integrated `<MarketContextPanel />` |
| `frontend/src/workspaces/ActivePositionWorkspace.tsx` | Integrated `<MarketContextPanel />` |
| `frontend/src/styles.css` | Added market context panel CSS |
| `tests/test_market_context_overlay.py` | 18 backend endpoint tests |

## Key Implementation Notes

### Endpoint Position
`GET /workspaces/market-context` must be registered BEFORE `GET /workspaces/{route_id}`
in workspace_router to avoid the dynamic segment capturing "market-context" as a
route_id. FastAPI's Starlette routing prefers static paths but the explicit ordering
makes the intent clear.

### Partial Failure Handling
`MarketSnapshotService.fetch_context` already handles partial provider failures
gracefully. The overlay endpoint returns 200 even when some symbols are unavailable
(is_partial=True). Only when all symbols fail is is_empty=True. This allows
workspace overlays to render partial context without crashing.

### Symbol Input: Frontend-Driven
The workspace projection does not yet extract ticker symbols from lifecycle event
payloads. `MarketContextPanel` includes a symbol text input (supports comma-separated).
This is explicit MVP scope — TF-0048 and TF-0049 will add contextual enrichment.

### Decimal Serialization
`snap.price.open` etc. are `Decimal` objects. Converted to `str(snap.price.open)`
for JSON serialization. The frontend receives these as strings (e.g., "185.00").
This preserves precision without float approximation.

### Default Provider Composition
`create_app()` now wires `MarketSnapshotService(YFinanceProvider())` as the
default `market_snapshot_service`. Tests inject `MarketSnapshotService` with a
mocked provider via the `market_snapshot_service` param.

## Tests Executed

- `uv run pytest tests/test_market_context_overlay.py` — 18 passed
- `uv run pytest` — 315 passed
- `uv run ruff check .` — clean
- `uv run mypy src tests` — clean
- `npm.cmd run typecheck` — clean
- `npm.cmd run lint` — clean
- `npm.cmd run build` — clean (238.97 kB JS bundle)

## Architectural Observations

TF-0047 is the first M9 issue that visibly connects market providers to workspace
surfaces. The data path is:
```
YFinanceProvider (infrastructure)
  → MarketSnapshotService (services)
    → GET /workspaces/market-context (app API)
      → fetchMarketContext (frontend API client)
        → MarketContextPanel (React component)
```

All three provider adapters (yfinance, Polygon, Alpaca) are compatible with this
pipeline — swapping providers requires only changing the `create_app()` argument.
This confirms ADR-0032's replaceability goal is met in practice.

## Follow-up Opportunities

- TF-0048: Market regime interpretation model (adds regime classification to overlays)
- TF-0049: Contextual operational summaries (combines workspace + market context)
- TF-0050: Provider provenance tracking registry
- Future: extract ticker from lifecycle event payload so panel pre-populates symbol
