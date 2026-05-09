---
title: LifecycleEvent
type: entity
status: canonical
tags: [TradeForge, entity, event-sourcing, decision-lifecycle]
created: 2026-05-08
updated: 2026-05-08
aliases: [Lifecycle Event]
---

# LifecycleEvent

## Definition

A LifecycleEvent is an immutable event-backed fact representing a workflow transition or lifecycle-relevant decision fact.

## Semantic Role

LifecycleEvent is the bridge between decision workflow semantics and the [[Event Ledger]].

## Authority Boundary

LifecycleEvent may only represent something that happened. It must not encode predictions, UI state, AI conclusions, or inferred quality judgments as facts.

## Lifecycle Relationship

LifecycleEvent records progression through:

```text
Idea -> Thesis -> Plan -> Approval -> Execution -> Position -> Review
```

Transitions must be explicit and governed by the [[Decision Lifecycle Engine]].

## Event Relationships

Related event examples:

- `TradeIdeaCreated`
- `ThesisCreated`
- `PlanCreated`
- `PlanApproved`
- `DecisionExecuted`
- `ReviewCompleted`

## Replay And Review Relevance

Replay depends on LifecycleEvents to reconstruct workflow state, pending decisions, approvals, and review history.

## Related Concepts

- [[Event Ledger]]
- [[DecisionLifecycleState]]
- [[LifecycleTransitionValidator]]
- [[TradeIdea]]
- [[TradeThesis]]
- [[TradePlan]]
- [[ReviewArtifact]]
