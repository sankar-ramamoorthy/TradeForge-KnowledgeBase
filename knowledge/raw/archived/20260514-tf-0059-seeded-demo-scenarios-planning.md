---
title: TF-0059 Planning Capture — Seeded Replayable Demo Scenarios
date: 2026-05-14
status: raw
type: planning-capture
issue: TF-0059
milestone: M10
---

# TF-0059 Planning Capture

## Issue Goal

Provide realistic named demo scenarios for demonstrations and replay workflows.

Acceptance criteria:
- Replay workspaces contain meaningful operational examples
- Demo scenarios illustrate workflow philosophy

## Current State (TF-0058 baseline)

TF-0058 implemented a single AAPL breakout demo seeded to the Plan stage.
The demo invite panel shows when the attention queue is empty and no lifecycle stage is active.
A single "Start Demo" button triggers `runDemoFlow` which calls: init + Thesis + Plan transitions.

The AAPL scenario lands on Plan Review workspace — user still needs to go through Approval, Execution, Position, Review manually.

Problem: The Replay Workspace shows an empty or minimal timeline until the user has traversed most stages. The demo doesn't make the Replay Workspace meaningful on first use.

## Architecture Interpretation

Task category: Workspace/Cognitive UX + Lifecycle initialization (frontend-only).

No backend changes required — the existing lifecycle API handles all stage transitions from Idea through Review. The demo infrastructure already established the pattern in TF-0058.

Key invariants:
- Demo does not bypass lifecycle rules — it uses the same API surface
- All demo transitions use the same lifecycle API as normal workflow (human decision sovereignty preserved)
- No canonical state mutation — demo is just pre-seeding the event ledger through API calls

## Design Decisions

### Multiple named scenarios vs. one deep scenario

Decision: Implement 4 named scenarios seeded to different lifecycle depths.
Rationale: Each scenario illustrates a different workflow philosophy aspect. Users can choose based on what they want to explore.

### Scenario depths

1. **AAPL Breakout Swing Trade** → Plan stage (existing depth)
   - User experiences: plan authorization, risk review
   - Lands on: Plan Review workspace

2. **TSLA Completed Lifecycle Review** → Review stage (full lifecycle)
   - User experiences: Replay Workspace with complete 7-stage timeline
   - Lands on: Replay workspace — addresses "Replay workspaces contain meaningful operational examples"

3. **NVDA Active Position Management** → Position stage
   - User experiences: Active position monitoring, thesis integrity
   - Lands on: Active Position workspace

4. **SPY Disciplined Exit Review** → Review stage
   - User experiences: review workflow, decision/outcome separation
   - Lands on: Review workspace

### Scenario selection UI

Replace single "Start Demo" button with a 2×2 scenario card grid.
Each card: symbol, scenario name, description, stage indicator, Start button.
Track `activeScenarioId` state to show loading on the clicked card only.

### ActiveDecisionRecord extension

Add `scenario_name?: string` field so the sidebar badge can show which scenario is active.
This is a backward-compatible addition — existing records without this field continue to work.

## Event Impact

Each scenario generates between 3 (Plan) and 7 (Review) lifecycle events in the event ledger.
All events are immutable and replayable.
Demo events use `provenance: { actor: "demo", source: "guided-demo-mode" }`.

## Replay Implications

The TSLA and SPY scenarios seed all 7 lifecycle stages, producing a full replay timeline.
This is the primary mechanism for making the Replay Workspace contain meaningful operational examples.

## Testing Strategy

Frontend-only changes — verify with:
- `npm run typecheck` — no TypeScript errors
- `npm run lint` — clean
- `npm run build` — clean

Existing Python tests are unaffected (no backend changes).

## Tradeoffs Considered

- More scenarios = more lifecycle events per demo session (in-memory store only, acceptable)
- Stage-seeding through API calls means each scenario takes 1-7 sequential API round-trips — acceptable for a demo flow; adds ~1-2s for full-lifecycle scenarios
- The `scenario_name` field on `ActiveDecisionRecord` is advisory metadata only, does not affect projection behavior

## Unresolved Questions

- Should scenario cards show estimated time to complete? (deferred — adds complexity)
- Should users be able to restart a demo after navigating away? (out of scope TF-0059, deferred TF-0060)
