---
title: Scenario
type: entity
status: canonical
tags: [TradeForge, entity, scenario-discovery]
created: 2026-05-08
updated: 2026-05-08
aliases: [Scenario]
---

# Scenario

## Definition

A Scenario is a structured hypothesis about a potential market opportunity or risk.

## Semantic Role

Scenario supports attention routing, opportunity discovery, risk discovery, and watchlist interpretation.

## Authority Boundary

Scenario is advisory. It is not a decision, trade signal, execution instruction, plan, or position.

## Lifecycle Relationship

Scenario may inform creation of a [[TradeIdea]], but it cannot bypass any lifecycle stage.

## Event Relationships

Related event examples:

- `ScenarioGenerated`
- `ScenarioRanked`
- `ScenarioInvalidated`
- `ScenarioPromotedToWatchlist`

Scenario events are advisory constructs, not decision authority.

## Replay And Review Relevance

Replay should show what scenarios were visible or ranked at the time of a decision without treating them as authoritative facts of intent.

## Related Concepts

- [[MarketContext]]
- [[TradeIdea]]
- [[Decision Lifecycle Engine]]
- [[Persona]]

