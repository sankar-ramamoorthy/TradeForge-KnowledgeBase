---
title: Guided Walkthrough Pattern
status: processed
type: processed-knowledge
created: 2026-05-14
updated: 2026-05-14
source_issues:
  - TF-0060
tags:
  - walkthrough
  - onboarding
  - demoability
  - lifecycle
  - frontend
  - ux-architecture
---

# Guided Walkthrough Pattern

## Purpose

The guided walkthrough is a step-by-step instructional mode that progresses through all
7 lifecycle stages with contextual explanation at each workspace. It is pedagogically
distinct from the demo scenario grid (TF-0059) — scenarios provide pre-seeded examples
to explore; the walkthrough provides a sequential teaching experience.

## Architecture

### State machine

A `WalkthroughSession` (localStorage: `tradeforge.walkthrough_session`) holds:
- `decision_id`, `symbol`, `persona_id`, `persona_version`, `workspace_id` — context
- `current_step_index` — which of the 7 steps is active
- `active` — whether walkthrough is running

Step definitions are static constants in `walkthrough.ts` (WALKTHROUGH_STEPS).

### Persistent panel injection

`WalkthroughPanel` is rendered from `App.tsx` as the first child of `WorkspaceLayout`'s
main area. This keeps all workspace components walkthrough-unaware — no modification to
individual workspace JSX required. The panel appears consistently above workspace content
across all navigation transitions.

### Advance mechanics

Each step has `transitionStages: string[]`. When the user clicks Continue:
1. `advanceWalkthroughStep()` fires sequential lifecycle API calls
2. Session step index increments
3. Navigation to `nextWorkspacePath`

Step 3 fires two transitions (Execution + Position) in one click — this is the only
multi-transition step, combining what would otherwise be two navigationally identical steps.

### Last-step exit sentinel

`nextWorkspacePath: null` is the sentinel for the last step. The advance handler
calls `handleExitWalkthrough()` instead of navigating.

## Session Lifecycle

- Created by `initWalkthrough()` in `walkthrough.ts`
- Persists across page refreshes
- Cleared by: `handleExitWalkthrough()`, `handleClearDecision()` (clears both together)

## Entry Point

A "Start Guided Walkthrough →" button appears in the `OperatingWorkspace` demo invite panel
(below the scenario grid, separated by a divider). It is only shown when `onStartWalkthrough`
prop is provided (currently always from App.tsx).

## Invariant Compliance

Walkthrough uses the same lifecycle API surface as normal workflow.
Provenance: `{ actor: "walkthrough", source: "guided-walkthrough" }`.
Human decision sovereignty is maintained — the walkthrough is pedagogical, not autonomous.

## Related

- [[demo-scenario-architecture]] — complementary pre-seeded demo scenarios
