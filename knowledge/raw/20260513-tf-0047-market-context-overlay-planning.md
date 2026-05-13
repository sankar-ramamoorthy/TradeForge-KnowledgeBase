---
title: TF-0047 Planning — Market Context Workspace Overlays
type: planning-capture
status: raw
issue: TF-0047
milestone: M9
created: 2026-05-13
---

# TF-0047 Planning: Market Context Workspace Overlays

## Issue Goal

Surface advisory market context (OHLCV price data from providers) within workspace
surfaces. First M9 issue that consumes all three provider adapters through the
MarketSnapshotService (TF-0043).

## Affected Layers

- infrastructure: `YFinanceProvider` wired as default in `create_app()`
- app: new endpoint + response models + service extractor
- services: existing `MarketSnapshotService` used as-is (no changes)
- frontend: new `MarketContextPanel` component + API types + workspace integration

## Key Design Decisions

### What "Overlay" Means
The overlay is supplementary ADVISORY data that appears alongside the workspace
projection — not replacing it. It shows OHLCV data with provider provenance,
labeled clearly as advisory and non-authoritative.

### Symbol Selection: Frontend-Driven
The workspace projection does not yet extract a ticker symbol from lifecycle event
payloads (not formalized in the domain model). Rather than adding this now, the
frontend provides a symbol input field. The user types the ticker relevant to their
workflow. This is appropriate MVP scope — TF-0047 is the overlay mechanism; TF-0048
and TF-0049 will add regime interpretation and contextual summaries.

### Which Workspaces Receive the Panel
- Opportunity Workspace: evaluating a candidate idea
- Active Position Workspace: supervising current exposure
(Plan Review workspace is a natural candidate but keeping scope minimal for MVP)

### Endpoint: GET /workspaces/market-context
Positioned BEFORE `GET /workspaces/{route_id}` in the router to avoid being
captured by the dynamic route. Accepts `symbols` query param (comma-separated).

### Default Provider: YFinanceProvider
Free, no API key required. The `create_app()` composition root wires
`MarketSnapshotService(YFinanceProvider())` as the default. Other providers
can be injected for production.

### ADR Impact
ADR-0032 already covers the provider boundary model and wiring decisions.
No new ADR required.

## Response Shape
```
{
  "authority": "advisory",
  "provider_id": "yfinance",
  "fetched_at": "...",
  "available": [
    {
      "symbol": "AAPL",
      "provider_id": "yfinance",
      "fetched_at": "...",
      "data_as_of": "...",
      "open": "185.00", "high": "187.50", "low": "184.20", "close": "186.40",
      "volume": 52000000,
      "regime": "unknown"
    }
  ],
  "unavailable_symbols": [],
  "is_complete": true, "is_partial": false, "is_empty": false
}
```

## Test Strategy
- Backend: FastAPI TestClient tests injecting mock `MarketSnapshotService`
  - success: single symbol returns complete snapshot
  - partial: one symbol unavailable, is_partial=true
  - empty: all unavailable, is_empty=true
  - 400: empty symbols param
- Frontend: build must pass (npm run build, typecheck, lint)

## Scope Boundary

- In: endpoint, response models, application wiring, MarketContextPanel component,
  integration in OpportunityWorkspace + ActivePositionWorkspace, CSS for panel
- Out: regime interpretation (TF-0048), contextual summaries (TF-0049),
  provenance tracking registry (TF-0050), symbol extraction from lifecycle payloads
