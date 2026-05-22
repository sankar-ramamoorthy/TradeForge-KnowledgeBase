---
title: TF-0060 Planning Capture — One-Click Operational Walkthrough
date: 2026-05-14
status: raw
type: planning-capture
issue: TF-0060
milestone: M10
---

# TF-0060 Planning Capture

## Issue Goal

A complete operational walkthrough launches from a single entry point.

Scope: one-click demo initialization, automatic scenario loading, operational workspace
initialization, guided workflow activation.

## Distinction from TF-0059

TF-0059: Pick a scenario → instantly seeds all stages to a target depth → land in workspace.
TF-0060: Click once → step-by-step guided tour through all 7 lifecycle stages with
contextual explanation at each workspace, advancing one stage per click.

TF-0059 is about providing realistic pre-seeded examples.
TF-0060 is about teaching the TradeForge workflow philosophy through active participation.

## Architecture Interpretation

Task category: Workspace/Cognitive UX + Lifecycle initialization (frontend-only).

No backend changes required. The lifecycle API already handles all transitions.
The walkthrough is a frontend state machine: session + step definitions + persistent panel.

Key invariants:
- Walkthrough uses same lifecycle API as normal workflow (human decision sovereignty)
- Walkthrough provenance: { actor: "walkthrough", source: "guided-walkthrough" }
- Walkthrough session persists across page refreshes (localStorage)

## Design Decisions

### Persistent WalkthroughPanel

A persistent panel rendered in App.tsx above the workspace content (first child of
workspace-main grid). Shows on all workspaces while a walkthrough is active.

This approach avoids modifying individual workspace components and gives a consistent
experience regardless of which workspace the user is on.

### Step Definition Structure

Each step is a WalkthroughStepDef with:
- workspacePath: where to navigate for this step
- title + explanation: educational content about the workspace/stage
- actionLabel: what the continue button says
- transitionStages[]: lifecycle stages to advance through on click (can be multiple)
- nextWorkspacePath: where to navigate after advancing (null = last step → exit)

7 steps covering all 7 lifecycle stages, grouped by workspace:
- Step 0: Operating (Idea → explain Operating, advance to Thesis, navigate to Opportunity)
- Step 1: Opportunity (Thesis → explain Opportunity, advance to Plan, navigate to Plan Review)
- Step 2: Plan Review (Plan → explain planning, advance to Approval, stay in Plan Review)
- Step 3: Plan Review (Approval → explain authorization, advance to Execution+Position, navigate to Active Position)
- Step 4: Active Position (Position → explain position management, advance to Review, navigate to Review)
- Step 5: Review (Review → explain review philosophy, navigate to Replay, no transition)
- Step 6: Replay (last → explain replay, "Finish Walkthrough" button exits)

### Step 3 multi-transition (Execution + Position)

Step 3 has transitionStages: ["Execution", "Position"] — two sequential transitions in one click.
This combines Execution and Position opening into one walkthrough advance because:
- They happen in the same workspace (Plan Review → Active Position)
- Separating them adds an extra step without adding educational value
- The UI label "Record Execution and Open Position →" makes the intent clear

### Walkthrough session (localStorage)

`tradeforge.walkthrough_session`:
- decision_id, symbol, persona_id, persona_version, workspace_id
- current_step_index, active flag

Session persists across page refreshes. If user refreshes, walkthrough resumes at last step.
This is important for the educational experience — users shouldn't lose their place.

### Entry point in OperatingWorkspace

Added below the demo scenario grid: a separator + "Prefer a step-by-step guided tour?
[Start Guided Walkthrough →]" button.

This keeps demo scenarios and the walkthrough as distinct options without competing.

### ActiveDecisionRecord for walkthrough decisions

Walkthrough decisions do NOT set is_demo: true. They do NOT set scenario_name.
The WalkthroughPanel itself is the clear walkthrough mode indicator.
The sidebar badge shows symbol and stage normally.

## Event Impact

Walkthrough decisions generate 7 lifecycle events (one per stage) across 6 advance clicks.
The final state (Review stage) produces a complete 7-event timeline in the Replay workspace.

## Testing Strategy

Frontend-only: typecheck + lint + build.
513 Python tests unaffected.

## Tradeoffs Considered

- Multi-stage advance in step 3 (Execution+Position): simpler UX vs. less granular education.
  Decision: combine them, since both happen "before you start watching the position."
- Walkthrough panel position (above vs. sidebar): above workspace gives more room for explanation.
  Decision: above workspace (first child of workspace-main grid).
- WalkthroughPanel styling: accent-bordered panel with step dots indicator.
