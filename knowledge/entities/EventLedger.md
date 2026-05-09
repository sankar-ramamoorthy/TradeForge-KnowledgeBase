---
title: EventLedger
type: entity
status: canonical
tags: [TradeForge, entity, event-sourcing, replayability]
created: 2026-05-09
updated: 2026-05-09
aliases: [Event Ledger]
related:
  - "[[LifecycleEvent]]"
  - "[[ReplaySession]]"
  - "[[EventStorePort]]"
  - "[[InMemoryEventStoreAdapter]]"
  - "[[EVENT_TAXONOMY]]"
  - "[[INVARIANTS]]"
---

# EventLedger

## Definition

The EventLedger is the canonical, immutable, append-only record of TradeForge events.

It is the source of truth for durable system state.

## Semantic Role

EventLedger preserves historical facts so lifecycle state, workspace projections, replay sessions, reviews, and derived views can be reconstructed.

It represents what happened, not what the system currently displays or interprets.

## Authority Boundary

EventLedger is canonical truth.

It is not:

- a projection
- a dashboard state store
- an AI memory store
- a broker state mirror
- a mutable current-state table
- an event-store adapter implementation

Historical correction must occur through future events, not mutation of prior events.

## Lifecycle Relationship

Decision lifecycle state is derived from event history recorded in the EventLedger.

The [[Decision Lifecycle Engine]] governs valid lifecycle transitions; the EventLedger records the resulting facts.

## Event Relationships

EventLedger contains events from the canonical event domains:

- `persona`
- `workspace`
- `market`
- `scenario`
- `decision`
- `execution`
- `review`
- `system`

## Replay And Review Relevance

Replay depends on ordered EventLedger history, deterministic rules, and optional historical snapshots.

Review artifacts depend on EventLedger history to reconstruct what was known, visible, pending, approved, executed, and reviewed.

## Related Concepts

- [[LifecycleEvent]]
- [[ReplaySession]]
- [[ReviewArtifact]]
- [[EventStorePort]]
- [[InMemoryEventStoreAdapter]]
- [[EVENT_TAXONOMY]]
