---
title: TradePlan
type: entity
status: canonical
tags: [TradeForge, entity, decision-lifecycle]
created: 2026-05-08
updated: 2026-05-08
aliases: [Trade Plan]
---

# TradePlan

## Definition

A TradePlan is a structured execution proposal derived from a [[TradeThesis]].

It defines intended entry logic, risk parameters, invalidation conditions, and execution intent.

## Semantic Role

TradePlan translates reasoning into a proposed controlled action while preserving human approval and lifecycle discipline.

## Authority Boundary

TradePlan is not an executed trade or active position. It requires approval before it can produce [[ExecutionIntent]] or [[PositionIntent]].

## Lifecycle Relationship

TradePlan sits between Thesis and Approval in the canonical lifecycle.

It cannot skip directly to Position.

## Event Relationships

Related event examples:

- `PlanCreated`
- `PlanApproved`
- `PlanRejected`

Plan events are decision-domain facts.

## Replay And Review Relevance

Replay must reconstruct plan terms and approval state so review can compare planned risk and invalidation logic against actual outcomes.

## Related Concepts

- [[TradeThesis]]
- [[PositionIntent]]
- [[ExecutionIntent]]
- [[ReviewArtifact]]

