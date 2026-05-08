---
title: ExecutionIntent
type: entity
status: canonical
tags: [TradeForge, entity, decision-lifecycle, execution]
created: 2026-05-08
updated: 2026-05-08
aliases: [Execution Intent]
---

# ExecutionIntent

## Definition

ExecutionIntent is the approved internal intent to interact with execution systems according to an approved [[TradePlan]].

## Semantic Role

ExecutionIntent separates internal authorization and intent from external broker events.

## Authority Boundary

ExecutionIntent is not an order, fill, position, or broker state. It cannot be created by AI or scenario discovery without lifecycle approval.

## Lifecycle Relationship

ExecutionIntent belongs after Approval and before or during Execution. It must remain traceable to the approved plan and human-controlled workflow.

## Event Relationships

ExecutionIntent relates to decision-domain approval facts and execution-domain events such as:

- `OrderSubmitted`
- `OrderModified`
- `OrderCancelled`
- `FillReceived`

Execution events reflect external reality, not internal intent.

## Replay And Review Relevance

Replay must distinguish what the operator intended to execute from what external systems actually reported.

## Related Concepts

- [[TradePlan]]
- [[PositionIntent]]
- [[LifecycleEvent]]
- [[Event Ledger]]

