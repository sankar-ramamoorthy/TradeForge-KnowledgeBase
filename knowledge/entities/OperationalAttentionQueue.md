---
title: Operational Attention Queue
type: entity
status: canonical
tags: [TradeForge, entity, derived-state, workspace, attention]
created: 2026-05-11
updated: 2026-05-11
aliases: [Operational Attention, Attention Queue]
related:
  - "[[Workspace]]"
  - "[[Projection]]"
  - "[[Persona]]"
  - "[[DecisionLifecycleState]]"
---

# Operational Attention Queue

## Definition

An Operational Attention Queue is a deterministic derived read model that identifies what requires human attention within a Persona Workspace.

## Semantic Role

The queue supports context-before-action UX by explaining active responsibilities, review needs, stale decision context, risk concerns, and workflow-relevant changes.

## Authority Boundary

An Operational Attention Queue is not canonical truth, lifecycle authority, execution permission, AI prioritization, or approval.

Queue output must remain source-linked and rebuildable from event-backed inputs, deterministic rules, workspace projections, and persona context.

## Persona Relationship

Persona context may shape ordering or emphasis through deterministic interpretation. It must not mutate source facts.

## Replay Relationship

Replay may reconstruct attention queues from historical event history and historical persona context. Replay must not depend on current queue output as truth.

## Related Concepts

- [[Workspace]]
- [[Projection]]
- [[WorkspaceSummary|Workspace Summary]]
- [[Persona]]
- [[DecisionLifecycleState]]
