---
title: Implemented TF-0012 Lifecycle Transition Validator
type: processed-synthesis
status: processed
tags: [TradeForge, runtime, TF-0012, decision-lifecycle, validation, replayability]
created: 2026-05-09
updated: 2026-05-09
related:
  - "[[DecisionLifecycleState]]"
  - "[[LifecycleEvent]]"
  - "[[EventLedger]]"
  - "[[ReplaySession]]"
  - "[[TF-0011 Lifecycle State Model]]"
source_history:
  - knowledge/raw/implemented-tf-0012-lifecycle-transition-validator-20260509.md processed and removed after synthesis
---

# Implemented TF-0012 Lifecycle Transition Validator

## Status

Processed implementation synthesis for TF-0012.

TF-0012 is implemented in the runtime repository on branch:

```text
feature/tf-0012-lifecycle-transition-validator
```

## Implementation Summary

Runtime now includes deterministic lifecycle transition validation under:

```text
src/domain/lifecycle/
```

The implementation adds:

- `ALLOWED_LIFECYCLE_TRANSITIONS`
- `LifecycleTransitionValidation`
- `validate_lifecycle_transition`

The validator accepts only adjacent canonical lifecycle progression:

```text
None -> Idea -> Thesis -> Plan -> Approval -> Execution -> Position -> Review
```

## Rejection Behavior

The validator rejects:

- initial transitions to anything other than `Idea`
- lifecycle shortcuts such as `Idea -> Position` and `Plan -> Position`
- repeated lifecycle stages
- lifecycle regressions
- transitions after `Review`

Validation returns an immutable result object describing the current stage, requested stage, expected stage, validity, and rejection reason when invalid.

## Architecture Boundary

TF-0012 remains domain-only. It does not append events, read from an event store, orchestrate services, call brokers, mutate UI state, or create event envelope fields.

The validator is deterministic and replay-compatible because it depends only on supplied derived lifecycle state and the requested next stage.

## Runtime Operational Sync

Runtime planning documents were synchronized:

- `DOCS/ISSUE_REGISTER.md` marks TF-0012 done.
- `DOCS/MILESTONE_ROADMAP.md` marks TF-0012 done under M3.

## Validation

Runtime validation completed successfully:

- `uv run pytest` passed with 56 tests.
- `uv run ruff check .` passed.
- `uv run mypy src tests` passed.

## Follow-On Work

TF-0013 should use this validator from the lifecycle orchestration service before appending accepted lifecycle events through the event store port.

## Contradictions

No contradiction was found between the implementation, issue scope, ADR 0002, ADR 0003, and TradeForge lifecycle doctrine.
