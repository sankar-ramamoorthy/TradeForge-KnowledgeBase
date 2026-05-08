---
title: Persona
type: entity
status: canonical
tags: [TradeForge, entity, persona]
created: 2026-05-08
updated: 2026-05-08
aliases: [Persona]
---

# Persona

## Definition

A Persona is a behavioral and cognitive mode that shapes interpretation, prioritization, risk framing, and workflow emphasis.

## Semantic Role

Persona gives TradeForge a scoped reasoning context for market interpretation, workspace behavior, and review cadence.

## Authority Boundary

Persona is not a user account, authentication identity, authorization model, or UI preference. It cannot mutate canonical state or bypass lifecycle rules.

## Lifecycle Relationship

Persona scopes workflow context and may influence prioritization, but lifecycle transitions remain controlled by the [[Decision Lifecycle Engine]].

## Event Relationships

Related event examples:

- `PersonaCreated`
- `PersonaActivated`
- `PersonaDeactivated`
- `PersonaSettingsUpdated`

Persona events define operational reasoning context.

## Replay And Review Relevance

Replay must preserve persona context so historical decisions can be interpreted under the same operational mode.

## Related Concepts

- [[Workspace]]
- [[MarketContext]]
- [[Scenario]]
- [[ReviewArtifact]]

