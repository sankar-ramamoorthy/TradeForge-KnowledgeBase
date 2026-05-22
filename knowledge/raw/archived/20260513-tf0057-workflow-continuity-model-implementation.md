---
title: TF-0057 Operational Workflow Continuity Model — Implementation
type: raw-implementation
status: raw
issue: TF-0057
milestone: M10
created: 2026-05-13
tags:
  - M10
  - UX
  - workflow-continuity
  - auto-navigation
  - implementation
---

# TF-0057: Operational Workflow Continuity Model — Implementation

## What Changed

### Auto-navigation after lifecycle transitions

OpportunityWorkspace "Develop Thesis" (Idea→Thesis):
  → navigates to /workspaces/plan-review with decision_id

PlanReviewWorkspace "Record Execution" (Approval→Execution):
  → navigates to /workspaces/active-position with decision_id

ActivePositionWorkspace "Begin Position Review" (Position→Review):
  → navigates to /workspaces/review with decision_id

Other transitions: reload projection in current workspace (no navigation).
Pattern: makeTransitionHandler(stage, nextHref?) — nextHref triggers onNavigateProgrammatic.

### Live stage indicator in sidebar badge

App.tsx: activeStage state + handleStageLoaded useCallback
All 5 workspaces: onStageLoaded?(stage) called in loadProjection .then()
ActiveDecisionBadge: activeStage? prop → .active-decision-stage pill (accent colored)

Badge now shows: AAPL / [Idea] → [Thesis] → ... as lifecycle advances.
Stage updates whenever any workspace (re)loads its projection.

### New props added
- OpportunityWorkspace: onNavigateProgrammatic?, onStageLoaded?
- PlanReviewWorkspace: onNavigateProgrammatic?, onStageLoaded?
- ActivePositionWorkspace: onNavigateProgrammatic?, onStageLoaded?
- OperatingWorkspace: onStageLoaded?
- ReviewWorkspace: onStageLoaded?

All optional — existing tests unaffected.

### CSS
.active-decision-stage: accent-colored pill badge in sidebar

## Result

The full lifecycle flow now feels like one continuous guided environment:
1. New Trade Idea → Operating Workspace (sidebar shows AAPL / [Idea])
2. Attention queue → go to Opportunity, progress strip shows Idea stage
3. "Develop Thesis" → auto-navigated to Plan Review (sidebar updates to [Thesis])
4. "Create Plan" → stays in Plan Review (sidebar [Plan])
5. "Authorize Plan" → stays in Plan Review (sidebar [Approval])
6. "Record Execution" → auto-navigated to Active Position (sidebar [Execution])
7. "Record Position Opened" → stays in Active Position (sidebar [Position])
8. "Begin Position Review" → auto-navigated to Review Workspace (sidebar [Review])

## Verification
- 513 tests passed (no backend changes)
- tsc, eslint, vite build: clean
- ruff, mypy: clean
