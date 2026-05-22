---
title: TF-F002 Synthesis — Armed Lifecycle Stage
type: processed-synthesis
status: processed
tags: [TradeForge, TF-F002, lifecycle, armed, awaiting-trigger, architectural]
created: 2026-05-15
updated: 2026-05-15
source_history:
  - knowledge/raw/first testing feedback 20260514.md
  - knowledge/raw/20260515-tf-f002-planning.md
related:
  - "[[Decision Lifecycle]]"
  - "[[Lifecycle Authority]]"
  - "[[Replayability Is Foundational]]"
  - "[[Human Decision Sovereignty]]"
  - "[[ADR-0002]]"
  - "[[ADR-0036]]"
issues:
  - TF-F002
---

# TF-F002 Synthesis — Armed Lifecycle Stage

## What the Feedback Identified

After plan approval, the system jumped directly to execution recording with no state
representing "authorized but awaiting trigger conditions." The SMH walkthrough exposed
this concretely: the plan required a daily close above 585, rising volume, and
breadth confirmation before entry — none of which could be declared or preserved.

## Root Cause

The lifecycle transition map mapped APPROVAL → EXECUTION directly. The moment of
authorization and the moment of execution commitment were treated as the same event,
which is architecturally incorrect for condition-dependent discretionary entries.

## Architectural Decision

ADR-0036 documents the decision: Armed is a mandatory lifecycle stage between Approval
and Execution. See `DOCS/adr/0036-armed-lifecycle-stage.md`.

New canonical lifecycle:
```
Idea → Thesis → Plan → Approval → Armed → Execution → Position → Review
```

Event: `decision.plan_armed` — carries `trigger_conditions: list[str]` in payload.

## What Changed

**Domain:**
- `LifecycleStage.ARMED = "Armed"` added to enum
- CANONICAL_LIFECYCLE_STAGES: 7 → 8 stages
- LIFECYCLE_EVENT_STAGE_MAP: `decision.plan_armed` → ARMED
- ALLOWED_LIFECYCLE_TRANSITIONS: APPROVAL→ARMED, ARMED→EXECUTION
- LIFECYCLE_STAGE_EVENT_TYPE_MAP: ARMED → `decision.plan_armed`

**API:**
- `POST /lifecycle/decisions/arm-plan` — creates `decision.plan_armed` event with trigger conditions
- `GET /lifecycle/decisions/{id}/arm` — reads declared trigger conditions

**Frontend:**
- LifecycleProgressStrip: 8 stages, Armed between Approval and Execution
- `workspaceRouting.ts`: Armed → active-position workspace
- PlanReviewWorkspace: Approval-stage action is now "Arm Plan" (ArmPlanModal)
- ArmPlanModal: new modal for declaring trigger conditions (trigger_conditions list)
- ActivePositionWorkspace: Armed-state panel shows declared conditions +
  "Confirm Trigger Met" button transitioning to Execution

**Tests:** 5 test files updated. 661 passing.

## Invariants Preserved

- [[Decision Lifecycle]] — new stage is explicit, event-backed, deterministic
- [[Replayability Is Foundational]] — trigger conditions are now a replayable
  canonical artifact; replay can reconstruct what conditions were declared
- [[Lifecycle Authority]] — lifecycle engine owns Armed state; no bypassing
- [[Human Decision Sovereignty]] — operator declares and confirms trigger conditions

## Feedback Loop Closure

The SMH scenario gap is resolved. The operator can now:
1. Authorize a plan → APPROVAL
2. Declare trigger conditions (daily close above 585, rising volume, breadth) → ARMED
3. Watch the declared conditions
4. Confirm trigger met → EXECUTION

The trigger conditions and the moment of confirmation are both replayable canonical events.

## Architectural Significance

This is the most significant structural change since the initial lifecycle engine (ADR-0002).
It reinforces the core TradeForge philosophy: disciplined decision-making requires
capturing not just what was decided, but under what conditions the decision was executed.
