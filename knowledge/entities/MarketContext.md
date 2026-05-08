---
title: MarketContext
type: entity
status: canonical
tags: [TradeForge, entity, market-intelligence]
created: 2026-05-08
updated: 2026-05-08
aliases: [Market Context]
---

# MarketContext

## Definition

MarketContext is interpreted situational context about market conditions, regimes, volatility, breadth, macro conditions, and thematic narratives.

## Semantic Role

MarketContext helps operators understand the environment in which scenarios, ideas, theses, and plans are evaluated.

## Authority Boundary

MarketContext is interpreted or inferred state. It is not a trade decision, market fact event, execution instruction, or canonical lifecycle state.

## Lifecycle Relationship

MarketContext may inform [[Scenario]], [[TradeIdea]], [[TradeThesis]], and [[TradePlan]], but it cannot advance lifecycle state.

## Event Relationships

Market observations may be recorded as market-domain events. MarketContext itself is an interpretation derived from observations and rules.

## Replay And Review Relevance

Replay should preserve or reconstruct the market context visible at the time of decision-making without using live market APIs.

## Related Concepts

- [[Scenario]]
- [[Persona]]
- [[Workspace]]
- [[ReplaySession]]

