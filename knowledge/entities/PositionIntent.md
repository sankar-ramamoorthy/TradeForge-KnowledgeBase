---
title: PositionIntent
type: entity
status: canonical
tags: [TradeForge, entity, decision-lifecycle, execution]
created: 2026-05-08
updated: 2026-05-08
aliases: [Position Intent]
---

# PositionIntent

## Definition

PositionIntent is the internal approved intent to create, modify, manage, or exit market exposure.

## Semantic Role

PositionIntent preserves operator intent about desired exposure separately from broker execution facts and current position projections.

## Authority Boundary

PositionIntent is not a broker order, fill, or active position. It does not prove that exposure exists.

## Lifecycle Relationship

PositionIntent may emerge only after a [[TradePlan]] is approved and before or during execution handling.

## Event Relationships

PositionIntent should be linked to decision-domain approval facts and later execution-domain feedback such as orders, fills, or position events.

## Replay And Review Relevance

Replay must distinguish intended exposure from actual execution and resulting position state.

## Related Concepts

- [[TradePlan]]
- [[ExecutionIntent]]
- [[LifecycleEvent]]
- [[ReviewArtifact]]

