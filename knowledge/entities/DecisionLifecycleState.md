---
title: DecisionLifecycleState
type: entity
status: canonical
tags: [TradeForge, entity, decision-lifecycle, replayability, derived-state]
created: 2026-05-09
updated: 2026-05-09
aliases: [Decision Lifecycle State, Lifecycle State]
related:
  - "[[LifecycleEvent]]"
  - "[[EventLedger]]"
  - "[[ReplaySession]]"
  - "[[TF-0011 Lifecycle State Model]]"
---

# DecisionLifecycleState

## Definition

DecisionLifecycleState is the derived current lifecycle stage of a decision
workflow, reconstructed from recognized lifecycle facts in ordered event
history.

It is derived state, not canonical truth.

## Canonical Stage Order

```text
Idea -> Thesis -> Plan -> Approval -> Execution -> Position -> Review
```

These stages must not be merged, skipped by derivation rules, or renamed
without explicit lifecycle doctrine updates.

## Semantic Role

DecisionLifecycleState gives the runtime a deterministic way to answer:

```text
What lifecycle stage does this event history currently imply?
```

It supports workflow continuity, replay reconstruction, decision queues, and
review surfaces.

## Authority Boundary

DecisionLifecycleState does not approve transitions, execute trades, write
events, or mutate lifecycle history.

Authority remains with:

- [[EventLedger]] for canonical historical facts
- [[Decision Lifecycle Engine]] for workflow authority
- deterministic transition validation for future accepted transitions

## Derivation Boundary

Lifecycle state is derived from ordered lifecycle-relevant events.

Recognized event facts may include:

- `decision.trade_idea_created`
- `decision.thesis_created`
- `decision.plan_created`
- `decision.plan_approved`
- `execution.order_submitted`
- `execution.position_opened`
- `review.review_completed`

Unrelated events do not affect lifecycle state.

## Validation Boundary

Deriving the latest lifecycle state is separate from validating whether the
event sequence was legal.

Invalid transition rejection belongs to lifecycle transition validation, not
the state derivation object itself.

## Replay And Review Relevance

Replay sessions use DecisionLifecycleState to reconstruct historical workflow
position from the Event Ledger. Review surfaces may use the derived state to
compare intended workflow progression against actual event history.

## Related Concepts

- [[LifecycleEvent]]
- [[EventLedger]]
- [[ReplaySession]]
- [[TradeIdea]]
- [[TradeThesis]]
- [[TradePlan]]
