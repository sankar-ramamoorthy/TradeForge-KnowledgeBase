---
title: Projection
type: entity
status: canonical
tags: [TradeForge, entity, projection, derived-state, replayability]
created: 2026-05-10
updated: 2026-05-10
aliases: [Read Model, Derived Projection]
related:
  - "[[Derived State]]"
  - "[[EventLedger]]"
  - "[[EventStorePort]]"
  - "[[ReplaySession]]"
  - "[[Workspace]]"
---

# Projection

## Definition

A Projection is a derived read model reconstructed from ordered Event Ledger history and deterministic projection rules.

It is discardable derived state, not canonical truth.

## Semantic Role

Projections make event-backed history usable for workspace surfaces, replay reconstruction, decision queues, exposure views, and operational summaries.

## Authority Boundary

A Projection does not own lifecycle state, approve decisions, create events, execute trades, define workspace truth, or replace the Event Ledger.

If projection output conflicts with event history, event history wins.

## Replay Relationship

Replay may consume projection logic when the projection can be rebuilt deterministically from historical events, deterministic rules, and optional historical snapshots.

Replay must not depend on mutable current projections as historical truth.

## Rebuild Relationship

Projection rebuild is deterministic orchestration that rereads ordered Event Ledger history and regenerates derived projection outputs.

Rebuild reports are operational artifacts. They may describe rebuild results, but they do not create canonical truth or lifecycle authority.

## Workspace Relationship

Workspace surfaces may display projections, but the surfaces remain derived views.

Projection persistence may be introduced for performance, but persisted projections remain rebuildable and non-authoritative.

## Related Concepts

- [[EventLedger]]
- [[Derived State]]
- [[ReplaySession]]
- [[Workspace]]
- [[DecisionLifecycleState]]
