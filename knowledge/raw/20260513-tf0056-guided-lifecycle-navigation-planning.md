---
title: TF-0056 Guided Lifecycle Navigation — Planning Capture
type: raw-planning
status: raw
issue: TF-0056
milestone: M10
branch: feature/tf-0056-guided-lifecycle-navigation
created: 2026-05-13
tags:
  - M10
  - UX
  - demoability
  - lifecycle
  - planning
---

# TF-0056: Guided Lifecycle Navigation — Planning Capture

## Current State

Every workspace shows lifecycle state as:
  "Current Lifecycle Stage: Idea [canonical]"

That raw label means nothing to a trader who doesn't know the system architecture.
There is no visual indication of where Idea sits in the full 7-stage journey.
There is no contextual explanation of what each stage means or what to do next.

## What TF-0056 Adds

### LifecycleProgressStrip
A compact horizontal 7-step tracker showing:
  ✓ completed stages  ●  current stage (highlighted)  ○ future stages
At a glance the trader sees: "I'm at Idea, I have 6 stages ahead of me."

### WorkflowGuidanceNote
A contextual info block immediately below the strip:
  - One sentence: what this stage means
  - One sentence: what to do here

Together these replace the raw `lifecycle-context` div in every workspace.

## Implementation Plan

### New file: frontend/src/workspaces/LifecycleProgress.tsx
- LIFECYCLE_STAGES: readonly tuple of 7 stage names
- STAGE_GUIDANCE: record of stage → {meaning, guidance}
- LifecycleProgressStrip: props currentStage: string | null
- WorkflowGuidanceNote: props currentStage: string | null

### Workspaces to update (replace lifecycle-context div)
- OperatingWorkspace.tsx
- OpportunityWorkspace.tsx
- PlanReviewWorkspace.tsx
- ActivePositionWorkspace.tsx
- ReviewWorkspace.tsx

### CSS
- .lifecycle-progress-strip, .lifecycle-step, .lifecycle-step-dot,
  .lifecycle-step-label, .lifecycle-step-connector
- .workflow-guidance-note, .workflow-guidance-meaning, .workflow-guidance-action
- State variants: [data-state="done"], [data-state="current"], [data-state="future"]

## ADR Checkpoint

No new ADR. Purely presentational — does not change lifecycle authority, events,
or any domain logic. Existing lifecycle system is unchanged.

## Tests

No new backend tests. Frontend: typecheck + lint + build.
