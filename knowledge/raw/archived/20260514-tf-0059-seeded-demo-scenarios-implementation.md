---
title: TF-0059 Implementation Capture — Seeded Replayable Demo Scenarios
date: 2026-05-14
status: raw
type: implementation-capture
issue: TF-0059
milestone: M10
---

# TF-0059 Implementation Capture

## Summary

Implemented 4 named demo scenarios with different lifecycle depths, replacing the single AAPL "Start Demo" button with a 2×2 scenario selection grid.

## Files Changed

- `frontend/src/demo.ts` — Complete rewrite: `DemoScenario` type, `DEMO_SCENARIOS` array (4 scenarios), updated `runDemoFlow(scenario, options)` signature
- `frontend/src/activeDecision.ts` — Added `scenario_name?: string` to `ActiveDecisionRecord`
- `frontend/src/workspaces/OperatingWorkspace.tsx` — Replaced single demo panel with scenario grid, added `activeScenarioId` state, updated `handleStartDemo(scenario)` signature
- `frontend/src/styles.css` — Added `.demo-scenario-panel`, `.demo-scenario-grid`, `.demo-scenario-card`, `.demo-stage-badge` + 4 stage-specific color variants

## Scenarios Implemented

| ID | Symbol | Name | Target Stage | Landing |
|----|--------|------|-------------|---------|
| aapl-breakout | AAPL | Breakout Swing Trade | Plan | /workspaces/plan-review |
| tsla-complete | TSLA | Completed Lifecycle Review | Review | /workspaces/replay |
| nvda-position | NVDA | Active Position Management | Position | /workspaces/active-position |
| spy-exit | SPY | Disciplined Exit Review | Review | /workspaces/review |

## Lifecycle Seeding per Target Stage

- Plan: init + Thesis + Plan (3 events)
- Approval: init + Thesis + Plan + Approval (4 events)
- Position: init + Thesis + Plan + Approval + Execution + Position (6 events)
- Review: init + Thesis + Plan + Approval + Execution + Position + Review (7 events)

## Tests Executed

- `npm.cmd run typecheck` — clean
- `npm.cmd run lint` — clean
- `npm.cmd run build` — clean (24.10 kB CSS, 255.17 kB JS)
- `uv run pytest` — 513 passed (no backend regressions)

## Replay Workspace Significance

TSLA (complete lifecycle) and SPY (complete lifecycle) scenarios now seed 7 lifecycle events each into the event ledger. After starting either scenario, the Replay Workspace shows a meaningful 7-entry timeline with Lifecycle events covering all stages from Idea through Review. This fulfills the acceptance criterion "Replay workspaces contain meaningful operational examples."

## Operational Lessons

- The `DemoScenario` type with `targetStage` + `landingPath` creates a clean extensibility pattern — future scenarios can be added to `DEMO_SCENARIOS` without changing any UI or navigation code.
- The `activeScenarioId` state allows per-card loading indicators without blocking the entire grid.
- Stage-specific CSS badge colors make scenario depth visually scannable at a glance.
- Backward compatibility preserved: `scenario_name` is optional on `ActiveDecisionRecord` — existing localStorage entries without this field continue to deserialize correctly.

## Unresolved

- Full lifecycle scenarios (TSLA, SPY) require 7 sequential API calls — adds ~1-2s latency during demo setup. Acceptable for demo use case. Could be parallelized in a future enhancement but lifecycle ordering is sequential by design.
- The `demo-invite-panel` CSS class remains in styles.css for backward reference but is no longer used in the JSX (replaced by `demo-scenario-panel`). Could be removed in a future cleanup pass.
