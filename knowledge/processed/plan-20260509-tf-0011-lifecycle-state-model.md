---
title: TF-0011 Lifecycle State Model
type: processed-synthesis
status: processed
tags: [TradeForge, runtime, TF-0011, decision-lifecycle, replayability]
created: 2026-05-09
updated: 2026-05-09
related:
  - "[[DecisionLifecycleState]]"
  - "[[LifecycleEvent]]"
  - "[[EventLedger]]"
  - "[[EventStorePort]]"
  - "[[ReplaySession]]"
  - "[[Development Replayability]]"
  - "[[INVARIANTS]]"
source_history:
  - knowledge/raw/plan-20260509-tf-0011-lifecycle-state-model.md processed and removed after synthesis
  - knowledge/raw/implemented-tf-0011-lifecycle-state-model-20260509.md processed and removed after synthesis
---

# TF-0011 Lifecycle State Model

## Status

Processed planning and implementation synthesis for TF-0011.

TF-0011 is now implemented in the runtime repository on branch:

```text
feature/tf-0011-lifecycle-state-model
```

Runtime operational state now reflects completion:

- `TradeForge/DOCS/ISSUE_REGISTER.md` marks TF-0011 as `Done`.
- `TradeForge/DOCS/MILESTONE_ROADMAP.md` marks M3 as `In Progress` and TF-0011 as done.

## Summary

TF-0011 defines a pure domain lifecycle state model for the canonical decision lifecycle:

```text
Idea -> Thesis -> Plan -> Approval -> Execution -> Position -> Review
```

The bounded runtime shape is:

- `LifecycleStage`
- `CANONICAL_LIFECYCLE_STAGES`
- `DecisionLifecycleState`
- `LIFECYCLE_EVENT_STAGE_MAP`
- `derive_lifecycle_state`

The model derives current lifecycle state from ordered `EventEnvelope` history. It does not introduce service orchestration, event store changes, UI behavior, broker behavior, AI behavior, or new event envelope fields.

## Event Mapping

Recognized lifecycle facts for TF-0011:

| Event type | Derived stage |
|---|---|
| `decision.trade_idea_created` | Idea |
| `decision.thesis_created` | Thesis |
| `decision.plan_created` | Plan |
| `decision.plan_approved` | Approval |
| `execution.order_submitted` | Execution |
| `execution.position_opened` | Position |
| `review.review_completed` | Review |

Unrelated events are ignored. Empty or unrelated history produces no lifecycle state.

The derivation function returns the latest recognized lifecycle fact in the ordered event history.

## Boundary Decision

TF-0011 derives state only. It does not validate whether the recognized event order is legal.

Invalid shortcut rejection belongs to TF-0012, which should implement deterministic lifecycle transition validation. This preserves the distinction between:

- state derivation from historical facts
- transition validation before new lifecycle facts are accepted
- orchestration that appends events through the event store port

## Semantic Alignment

The TF-0011 implementation aligns with TradeForge doctrine:

- [[INVARIANTS]]: lifecycle state is event-backed, explicit, and replayable.
- [[EVENT_TAXONOMY]]: recognized lifecycle facts remain events, not interpretations.
- [[ARCHITECTURE]]: the Decision Lifecycle Engine owns workflow state while the domain model provides deterministic state derivation.
- ADR 0002: lifecycle authority remains centralized and ordered.
- ADR 0003: event domains remain aligned with the canonical taxonomy.

No new doctrine or invariant is required.

## Replay Implications

Lifecycle state is reconstructable from event history and deterministic rules. This supports replay by making the current lifecycle stage a derived view over the Event Ledger rather than mutable stored truth.

Replay-safe expectations:

- same ordered event history returns the same lifecycle state.
- unrelated events do not affect lifecycle state.
- no live API, projection cache, UI state, or AI output is required.
- invalid historical sequences are representable until TF-0012 adds validation for future transitions.

## Ontology Implications

TF-0011 stabilizes [[DecisionLifecycleState]] as a derived state object.

Stable distinctions:

- [[LifecycleEvent]] records lifecycle-relevant facts.
- [[DecisionLifecycleState]] is derived from recognized lifecycle facts.
- [[EventLedger]] remains canonical historical truth.
- [[ReplaySession]] reconstructs lifecycle state from event history.
- validation of allowed transitions is separate from derivation.

This avoids treating derived state as canonical truth while still giving the runtime a deterministic lifecycle state model.

## Workflow Implications

The M3 lifecycle sequence remains:

```text
TF-0011 derive lifecycle state
    -> TF-0012 validate lifecycle transitions
    -> TF-0013 orchestrate valid transitions and event appends
```

TF-0011 includes tests for:

- exact canonical stage order
- no merged lifecycle stages
- derivation for each recognized lifecycle event
- unrelated events ignored
- empty history returns no lifecycle state
- domain-layer dependency boundaries

## ADR Implications

No new ADR is required for TF-0011.

Existing ADR coverage is sufficient:

- ADR 0002 defines lifecycle authority and canonical stage order.
- ADR 0003 defines canonical event taxonomy and event-domain boundaries.

An ADR may become necessary later if lifecycle validation, orchestration, or replay behavior changes the authority boundary.

## Runtime Implementation

Runtime files added or updated:

- `src/domain/lifecycle/__init__.py`
- `src/domain/lifecycle/state.py`
- `tests/test_lifecycle_state.py`
- `DOCS/ISSUE_REGISTER.md`
- `DOCS/MILESTONE_ROADMAP.md`

## Validation

Current runtime validation completed successfully:

- `uv run pytest` passed with 28 tests.
- `uv run ruff check .` passed.
- `uv run mypy src tests` passed.

## Follow-On Work

TF-0012 should implement deterministic lifecycle transition validation, including invalid shortcut rejection.

TF-0013 should introduce lifecycle orchestration only after the domain validator exists.

## Contradictions

No contradiction was found between the TF-0011 implementation, issue scope, ADR 0002, ADR 0003, and TradeForge lifecycle doctrine.
