---
title: LifecycleOrchestrationService
type: entity
status: canonical
tags: [TradeForge, entity, decision-lifecycle, services, event-sourcing]
created: 2026-05-09
updated: 2026-05-09
aliases: [Lifecycle Orchestration Service]
related:
  - "[[DecisionLifecycleState]]"
  - "[[LifecycleTransitionValidator]]"
  - "[[LifecycleEvent]]"
  - "[[EventStorePort]]"
  - "[[EventLedger]]"
  - "[[TF-0013 Lifecycle Orchestration Service]]"
  - "[[Implemented TF-0013 Lifecycle Orchestration Service]]"
---

# LifecycleOrchestrationService

## Definition

LifecycleOrchestrationService is the services-layer coordinator that handles
lifecycle transition requests by combining event-derived state, deterministic
transition validation, and append-only event storage through the
[[EventStorePort]].

It orchestrates workflow mechanics. It does not define lifecycle rules.

## Responsibility

The service coordinates this sequence:

```text
EventStore.read_events()
    -> derive DecisionLifecycleState
    -> validate requested transition
    -> append LifecycleEvent only when valid
```

## Authority Boundary

LifecycleOrchestrationService may coordinate accepted lifecycle transitions, but
it must not become the semantic owner of lifecycle rules.

Authority remains split:

- [[EventLedger]] owns canonical historical facts.
- [[DecisionLifecycleState]] derives current lifecycle state from event history.
- [[LifecycleTransitionValidator]] owns deterministic transition validation.
- [[EventStorePort]] provides append/read access without defining persistence
  semantics.

## Invalid Transition Boundary

Invalid transitions must not append events.

The service should return validation information to callers without mutating
event history when the requested transition is invalid.

## Layer Boundary

The service may depend on domain objects and ports. It must not depend directly
on infrastructure adapters, UI, brokers, live trading systems, or AI outputs.

## Related Concepts

- [[DecisionLifecycleState]]
- [[LifecycleTransitionValidator]]
- [[LifecycleEvent]]
- [[EventStorePort]]
- [[EventLedger]]
