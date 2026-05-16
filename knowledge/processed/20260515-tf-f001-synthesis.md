---
title: TF-F001 Synthesis — Iterative Revision Workflow
type: processed-synthesis
status: processed
tags: [TradeForge, TF-F001, revision-workflow, plan-revision, thesis-revision, feedback-loop]
created: 2026-05-15
updated: 2026-05-15
source_history:
  - knowledge/raw/first testing feedback 20260514.md
  - knowledge/raw/20260515-tf-f001-planning.md
related:
  - "[[Decision Lifecycle]]"
  - "[[Replayability Is Foundational]]"
  - "[[Human Decision Sovereignty]]"
  - "[[UX Is Architectural]]"
issues:
  - TF-F001
---

# TF-F001 Synthesis — Iterative Revision Workflow

## What the Feedback Identified

First operational walkthrough (M10A, 2026-05-14) showed that when the advisory system
surfaced gaps during plan review (only 1 invalidation condition, only 1 execution assumption),
there was no revision path available. The operator could only proceed or abandon.

The workflow was forward-only.

## Root Cause

Thesis revision was implemented end-to-end for the THESIS stage only.
Plan revision was never implemented.
The thesis revision stage check was also too restrictive — it blocked revision
once the operator moved from the Thesis stage to the Plan stage.

Two gaps, same root: incomplete application of the revision pattern.

## What Changed

**Backend (`src/app/api/routes.py`):**
- `POST /lifecycle/decisions/revise-plan` — new endpoint creating immutable
  `decision.plan_revised` events (mirrors `revise-thesis` exactly)
- `GET /lifecycle/decisions/{id}/plan/history` — full plan revision history
- `GET /lifecycle/decisions/{id}/plan` — updated to return latest revision
  (scans both `plan_created` and `plan_revised`)
- `POST /lifecycle/decisions/revise-thesis` — stage check relaxed from
  THESIS-only to THESIS or PLAN

**Frontend:**
- `PlanRevisionModal.tsx` — new revision modal pre-filled from current plan
- `PlanReviewWorkspace` — "Revise Plan" button visible at Plan stage;
  "Revise Thesis" button now visible at both Thesis and Plan stages

## Invariants Preserved

- [[Replayability Is Foundational]] — revisions create new immutable events, never overwrite
- [[Human Decision Sovereignty]] — operator can revise without system blocking
- [[Decision Lifecycle]] — revision is not a lifecycle transition; stage remains unchanged

## Verification

657 tests passing (no regressions). Typecheck clean. Build clean.

## Feedback Loop Closure

The specific scenario that surfaced the gap (advisory items with no revision path) is
now resolved. The operator can revise both thesis and plan from the Plan Review workspace
without abandoning the decision.

## Follow-on Gaps Observed

None from this fix. TF-F002 and TF-F003 remain as independent feedback issues.
