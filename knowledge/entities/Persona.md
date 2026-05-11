---
title: Persona
type: entity
status: canonical
tags: [TradeForge, entity, persona]
created: 2026-05-08
updated: 2026-05-11
aliases: [Persona]
---

# Persona

## Definition

A Persona is a behavioral and cognitive mode that shapes interpretation, prioritization, risk framing, and workflow emphasis.

Runtime persona context may include explicit version identity plus workspace, workflow, and decision context references so derived systems can apply persona influence without changing semantic authority.

## Semantic Role

Persona gives TradeForge a scoped reasoning context for market interpretation, workspace behavior, and review cadence.

## Authority Boundary

Persona is not a user account, authentication identity, authorization model, or UI preference. It cannot mutate canonical state or bypass lifecycle rules.

Persona influence is bounded interpretive scope, not executable behavior.

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

Persona evolution must remain versioned so later persona changes do not silently reinterpret prior workflows, workspaces, or reviews.

## Related Concepts

- [[Workspace]]
- [[MarketContext]]
- [[Scenario]]
- [[ReviewArtifact]]
