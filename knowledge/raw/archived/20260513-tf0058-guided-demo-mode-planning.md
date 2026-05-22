---
title: TF-0058 Guided Demo Mode — Planning Capture
type: raw-planning
status: raw
issue: TF-0058
milestone: M10
branch: feature/tf-0058-guided-demo-mode
created: 2026-05-13
tags:
  - M10
  - demoability
  - demo
  - seeded-flow
  - planning
---

# TF-0058: Guided Demo Mode — Planning Capture

## Design

### Seeded scenario (AAPL breakout)

Fires 3 API calls in sequence:
1. POST /lifecycle/decisions/init — symbol: AAPL, realistic thesis text
2. POST /lifecycle/transitions — Thesis stage, seeded thesis payload
3. POST /lifecycle/transitions — Plan stage, seeded plan payload

Lands user at Plan Review Workspace at Plan stage.
Progress strip shows ✓Idea ✓Thesis ●Plan — visually rich starting point.
User then interacts normally: Authorize Plan → Record Execution → etc.
No backend changes — uses existing API surface.

### Entry point

OperatingWorkspace empty-queue state.
Currently: "No pending operational attention items" + ✓ icon.
With TF-0058: inviting DemoInvitePanel when queue empty AND no active decision.
Shows: scenario description + "Start Demo" button.
Loading state: "Setting up demo scenario…" during the 3-call sequence.
On success: setActiveDecision with is_demo: true, navigate to Plan Review.

### Demo indicator

activeDecision.ts: add is_demo?: boolean to ActiveDecisionRecord.
ActiveDecisionBadge: shows "Demo" tag when is_demo is true.
No per-workspace changes needed — badge is the single visibility point.

### Operational explanation overlays

The DemoInvitePanel in Operating explains what will happen before the demo starts.
After demo starts, the LifecycleProgressStrip (TF-0056) + WorkflowGuidanceNote
(TF-0056) already provide stage-aware operational context.
These together satisfy "operational explanation overlays" for this MVP scope.

## ADR Checkpoint

No new ADR. Demo mode is a UX initialization pattern using the existing API surface.
Seeded payloads are canonical lifecycle events — same as manual entry.
is_demo flag is a client-side session marker, not canonical state.

## Files

New: frontend/src/demo.ts
Modified: frontend/src/activeDecision.ts, OperatingWorkspace.tsx, operationalLayout.tsx, styles.css

## Tests

No new backend tests. Frontend: typecheck + lint + build.
Existing tests pass — is_demo is optional, no behavior change in tests.
