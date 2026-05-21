---
title: TF-0057 Operational Workflow Continuity Model — Planning Capture
type: raw-planning
status: raw
issue: TF-0057
milestone: M10
branch: feature/tf-0057-workflow-continuity-model
created: 2026-05-13
tags:
  - M10
  - UX
  - workflow-continuity
  - auto-navigation
  - planning
---

# TF-0057: Operational Workflow Continuity Model — Planning Capture

## Current State

After a lifecycle action succeeds in any workspace, the workspace reloads its
own projection and stays put. The user must manually navigate to the next
workspace. The sidebar badge shows the symbol but not the stage.

## Two Deliverables

### 1. Post-transition auto-navigation

3 transitions trigger automatic routing to the logical next workspace:

| Action | Old Stage | New Stage | Navigate To |
|---|---|---|---|
| Develop Thesis | Idea | Thesis | Plan Review |
| Record Execution | Approval | Execution | Active Position |
| Begin Position Review | Position | Review | Review Workspace |

Other transitions stay in the current workspace (logical — you're still working there).

### 2. Live stage indicator in sidebar badge

ActiveDecisionBadge gains activeStage? prop.
App.tsx holds activeStage state, updated via handleStageLoaded useCallback.
All 5 workspaces call onStageLoaded(stage) after projection loads.
Badge shows: AAPL / Idea → AAPL / Thesis → ... as lifecycle progresses.

## Props added to workspaces

- All 5: onStageLoaded?: (stage: string | null) => void
- Opportunity, PlanReview, ActivePosition: onNavigateProgrammatic?: (href: string) => void

onStageLoaded called in loadProjection's .then() — intentionally omitted from
useCallback deps (stable callback, eslint-disable comment already present).

Auto-navigation: build URL directly as:
  path + (decision_id ? "?decision_id=" + encodeURIComponent(decision_id) : "")

Do NOT reload projection after auto-nav — the new workspace loads its own.

## ADR Checkpoint

No new ADR. Workflow routing is a UX behavior, not an architectural boundary.
Auto-navigation derives from the deterministic stage-to-workspace mapping,
consistent with INVARIANTS.md (workflow-centric, not dashboard-centric).

## Tests

No new backend tests. Frontend: typecheck + lint + build.
Existing 513 tests unaffected (optional props, no behavior change in tests).
