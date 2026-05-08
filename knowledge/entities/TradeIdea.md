---
title: TradeIdea
type: entity
status: canonical
tags: [TradeForge, entity, decision-lifecycle]
created: 2026-05-08
updated: 2026-05-08
aliases: [Trade Idea]
---

# TradeIdea

## Definition

A TradeIdea is the earliest lifecycle hypothesis about a potential market opportunity or risk.

It is unapproved, non-executable, and not yet structured enough to become a plan.

## Semantic Role

TradeIdea captures initial operator attention, scenario intake, or observed opportunity before formal reasoning exists.

## Authority Boundary

TradeIdea is not a decision, trade, position, order, or execution instruction. It cannot authorize exposure or bypass the [[Decision Lifecycle Engine]].

## Lifecycle Relationship

TradeIdea is the first stage in the canonical lifecycle:

```text
Idea -> Thesis -> Plan -> Approval -> Execution -> Position -> Review
```

It may progress only by becoming a [[TradeThesis]] through explicit lifecycle transition.

## Event Relationships

Related event examples:

- `TradeIdeaCreated`
- `DecisionQueued`

TradeIdea events are decision-domain facts and must remain event-backed.

## Replay And Review Relevance

Replay must reconstruct when the idea emerged, what context was visible, and whether it progressed, stalled, or was abandoned.

## Related Concepts

- [[TradeThesis]]
- [[Scenario]]
- [[Decision Lifecycle Engine]]
- [[Event Ledger]]

