---
title: Workspace
type: entity
status: canonical
tags: [TradeForge, entity, workspace, UX]
created: 2026-05-08
updated: 2026-05-11
aliases: [Persona Workspace, Operational Workspace]
---

# Workspace

## Definition

A Workspace is a persona-scoped operational decision environment.

It is not a UI tab, dashboard, or generic page.

## Semantic Role

Workspace structures situational awareness, active exposure, opportunity attention, decision queues, and review context.

## Authority Boundary

Workspace surfaces are projections. They do not own canonical state, approve decisions, execute trades, or mutate lifecycle state.

## Lifecycle Relationship

Workspace exposes lifecycle context and pending decisions, but lifecycle progression remains owned by the [[Decision Lifecycle Engine]].

## Projection Relationship

Workspace state is expressed through rebuildable [[Projection]] output.

The M6 Persona Workspace projection layer stabilizes three derived workspace read-model forms:

- workspace projection read models
- [[OperationalAttentionQueue|Operational Attention Queue]]
- [[WorkspaceSummary|Workspace Summary]]

These forms support operational cognition without becoming workspace-owned truth.

## Event Relationships

Related event examples:

- `WorkspaceCreated`
- `WorkspaceOpened`
- `WorkspaceClosed`
- `WorkspaceContextUpdated`

Workspace events track operational context, not UI state as domain truth.

## Replay And Review Relevance

Replay should reconstruct the workspace context visible during decision-making from events, deterministic rules, and historical snapshots.

## Related Concepts

- [[Persona]]
- [[Projection]]
- [[OperationalAttentionQueue|Operational Attention Queue]]
- [[WorkspaceSummary|Workspace Summary]]
- [[ReplaySession]]
- [[ReviewArtifact]]
- [[MarketContext]]
