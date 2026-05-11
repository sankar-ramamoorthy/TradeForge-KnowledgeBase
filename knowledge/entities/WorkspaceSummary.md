---
title: Workspace Summary
type: entity
status: canonical
tags: [TradeForge, entity, derived-state, workspace, summary]
created: 2026-05-11
updated: 2026-05-11
aliases: [Context-Aware Workspace Summary]
related:
  - "[[Workspace]]"
  - "[[Projection]]"
  - "[[OperationalAttentionQueue|Operational Attention Queue]]"
  - "[[Persona]]"
---

# Workspace Summary

## Definition

A Workspace Summary is a deterministic, non-authoritative derived read model that condenses workspace projection state and operational attention into concise workspace context.

## Semantic Role

Workspace Summary output helps workspace surfaces communicate current context, emphasis, source inputs, source event references, and attention references.

## Authority Boundary

A Workspace Summary is not canonical truth, lifecycle authority, execution permission, AI-generated interpretation, or a substitute for source event history.

Summaries must preserve source traceability and remain rebuildable.

## Persona Relationship

Persona context may shape emphasis. Persona-shaped emphasis remains interpretive and must not rewrite facts or conceal uncertainty.

## Replay Relationship

Replay may reconstruct workspace summaries from historical workspace projections, attention queues, event history, deterministic rules, and persona context.

## Related Concepts

- [[Workspace]]
- [[Projection]]
- [[OperationalAttentionQueue|Operational Attention Queue]]
- [[Persona]]
