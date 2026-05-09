---
title: LifecycleTransitionValidator
type: entity
status: canonical
tags: [TradeForge, entity, decision-lifecycle, validation, replayability]
created: 2026-05-09
updated: 2026-05-09
aliases: [Lifecycle Transition Validator]
related:
  - "[[DecisionLifecycleState]]"
  - "[[LifecycleEvent]]"
  - "[[EventLedger]]"
  - "[[ReplaySession]]"
  - "[[TF-0012 Lifecycle Transition Validator]]"
  - "[[Implemented TF-0012 Lifecycle Transition Validator]]"
---

# LifecycleTransitionValidator

## Definition

LifecycleTransitionValidator is the deterministic domain rule component that
evaluates whether a requested lifecycle stage may follow the current
[[DecisionLifecycleState]].

It validates proposed progression. It does not create events, append to the
Event Ledger, orchestrate services, or mutate workflow history.

## Canonical Transition Order

```text
None -> Idea -> Thesis -> Plan -> Approval -> Execution -> Position -> Review
```

Only adjacent forward progression is valid.

## Rejection Semantics

The validator rejects:

- initial transitions to any stage other than Idea
- skipped stages
- repeated stages
- regressions
- transitions after Review

Invalid transition rejection prevents lifecycle shortcuts from becoming
canonical workflow facts.

## Authority Boundary

LifecycleTransitionValidator is part of lifecycle rule evaluation. It is not
the Event Ledger and is not service orchestration.

It answers:

```text
Is this requested next lifecycle stage allowed from the current derived state?
```

It does not answer:

```text
Should an event be appended?
Who requested the transition?
What persistence adapter should store the event?
```

Those concerns belong to orchestration and event storage boundaries.

## Replay Relevance

The validator is replay-compatible because it depends only on deterministic
stage order, supplied current lifecycle state, and requested next stage. It must
not depend on live APIs, UI state, broker state, projections, or AI output.

## Related Concepts

- [[DecisionLifecycleState]]
- [[LifecycleEvent]]
- [[EventLedger]]
- [[ReplaySession]]
