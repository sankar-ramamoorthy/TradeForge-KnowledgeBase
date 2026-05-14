---
title: TF-0056 Guided Lifecycle Navigation — Implementation
type: raw-implementation
status: raw
issue: TF-0056
milestone: M10
created: 2026-05-13
tags:
  - M10
  - UX
  - lifecycle
  - implementation
---

# TF-0056: Guided Lifecycle Navigation — Implementation

## What Changed

### New file: frontend/src/workspaces/LifecycleProgress.tsx
- LIFECYCLE_STAGES: readonly tuple ["Idea","Thesis","Plan","Approval","Execution","Position","Review"]
- STAGE_GUIDANCE: stage → {meaning, guidance} for all 7 stages
- LifecycleProgressStrip: renders compact 7-step horizontal tracker
  - done steps: green filled dot with ✓
  - current step: outlined green dot with box-shadow ring + bold label
  - future steps: grey dot + muted label
  - connector lines between dots colored by done/future state
  - handles null currentStage silently (returns null)
- WorkflowGuidanceNote: renders meaning + guidance text below the strip
  - accent-colored left border, accent-surface background
  - handles null and unknown stages silently

### Updated 5 workspace files
- OperatingWorkspace.tsx
- OpportunityWorkspace.tsx
- PlanReviewWorkspace.tsx
- ActivePositionWorkspace.tsx
- ReviewWorkspace.tsx

Each: import added, lifecycle-context div replaced with:
  <LifecycleProgressStrip currentStage={lifecycleStage} />
  <WorkflowGuidanceNote currentStage={lifecycleStage} />

### styles.css additions
.lifecycle-progress-strip, .lifecycle-step, .lifecycle-step-connector,
.lifecycle-step-dot, .lifecycle-step-label (with [data-state] variants),
.workflow-guidance-note, .workflow-guidance-meaning, .workflow-guidance-action

## Result

Every workspace now shows:
1. Where you are in the 7-stage journey (visual progress strip)
2. What this stage means (one sentence)
3. What to do here (one sentence guidance)

No architectural changes. No domain logic touched. Purely presentational.

## Verification
- 513 tests passed (no backend changes)
- tsc, eslint, vite build: clean
