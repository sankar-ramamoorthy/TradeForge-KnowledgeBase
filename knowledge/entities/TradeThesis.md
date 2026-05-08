---
title: TradeThesis
type: entity
status: canonical
tags: [TradeForge, entity, decision-lifecycle]
created: 2026-05-08
updated: 2026-05-08
aliases: [Thesis, Trade Thesis]
---

# TradeThesis

## Definition

A TradeThesis is a structured rationale for a potential trade derived from a [[TradeIdea]].

It records reasoning, assumptions, relevant context, supporting signals, and uncertainty.

## Semantic Role

TradeThesis turns initial attention into an explicit argument that can be evaluated before planning.

## Authority Boundary

TradeThesis is not executable. It does not authorize orders, positions, or broker interaction.

## Lifecycle Relationship

TradeThesis sits between [[TradeIdea]] and [[TradePlan]] in the decision lifecycle.

It may progress only when a plan is explicitly created from it.

## Event Relationships

Related event examples:

- `ThesisCreated`
- `DecisionQueued`

Thesis events are decision-domain facts controlled by the [[Decision Lifecycle Engine]].

## Replay And Review Relevance

Replay must preserve the thesis that existed before planning so later review can compare original reasoning against outcome.

## Related Concepts

- [[TradeIdea]]
- [[TradePlan]]
- [[MarketContext]]
- [[ReviewArtifact]]

