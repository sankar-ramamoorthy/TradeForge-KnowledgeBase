---
title: TF-0049 Planning — Contextual Operational Summaries
type: planning-capture
status: raw
issue: TF-0049
milestone: M9
created: 2026-05-13
---

# TF-0049 Planning: Contextual Operational Summaries

## Issue Goal

Produce combined workspace operational state + market context summaries.
TF-0023 summaries are event-sourced but market-blind. TF-0047/0048 add
market data and regime. TF-0049 stitches them together into a unified
advisory operational briefing for the operating workspace.

## Design

### Service: ContextualSummaryService
- Wraps WorkspaceSummaryReadService + optional MarketSnapshotService
- `summarize_for(persona_context, symbols)` → ContextualOperationalSummary
- Fetches workspace summary (from event history) and market context (from provider)
- Market context is optional; absent symbols → market notes empty
- Authority: always "derived" (workspace summary) + "advisory" (market notes)

### Data Model
- `MarketContextNote`: symbol, close price string, regime, provider_id, data_as_of
- `ContextualOperationalSummary`: operational headline + details + market notes
- All fields immutable frozen dataclasses

### API Endpoint
- `GET /workspaces/contextual-summary` — registered before `/{route_id}`
- Query: workspace context params + optional `symbols` (comma-separated)
- Response: headline + details + market notes, labeled derived/advisory
- No mutation; no lifecycle authority

### Frontend
- New `ContextualBriefingPanel` component in OperatingWorkspace
- Symbol input → fetch → show headline + market notes together
- Reuses CSS from MarketContextPanel

### Application Wiring
- `create_app()`: extract market_svc first, reuse for both
  market_snapshot_service and ContextualSummaryService (avoid double instantiation)

## ADR Impact
None. Combines existing patterns from TF-0023 and TF-0047.

## Scope Boundary
- In: service + data models, API endpoint, frontend briefing panel
- Out: AI-generated summaries, regime storage, multi-workspace contextual summary
