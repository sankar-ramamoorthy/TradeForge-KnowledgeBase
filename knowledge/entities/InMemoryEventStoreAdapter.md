---
title: InMemoryEventStoreAdapter
type: entity
status: draft
tags: [TradeForge, entity, runtime, infrastructure, event-sourcing]
created: 2026-05-09
updated: 2026-05-09
aliases: [In-Memory Event Store, In-Memory Event Store Adapter]
related:
  - "[[EventStorePort]]"
  - "[[EventLedger]]"
  - "[[ReplaySession]]"
  - "[[TF-0010 In-Memory Event Store]]"
---

# InMemoryEventStoreAdapter

## Definition

An InMemoryEventStoreAdapter is a runtime infrastructure adapter that implements the [[EventStorePort]] contract using process-local memory.

It exists for tests, local development, and early vertical slices.

## Semantic Role

The adapter demonstrates append-only event-store behavior without introducing a durable persistence strategy.

It supports Event Ledger doctrine operationally, but it does not define canonical event semantics.

## Authority Boundary

InMemoryEventStoreAdapter is not the [[EventLedger]].

It must not:

- become durable persistence doctrine
- imply distributed event streaming architecture
- expose update, delete, overwrite, or truncation operations
- become lifecycle authority
- become projection-authored truth
- depend on live APIs or broker state

## Workflow Relationship

Runtime workflows may use this adapter for tests and early vertical slices that need append-only event history.

Lifecycle validation must occur before appending lifecycle events. The adapter records events accepted by the relevant domain or service workflow; it does not validate lifecycle semantics by itself.

## Replay And Review Relevance

The adapter supports deterministic append-order reads for local replay and projection tests.

Replay correctness still depends on event integrity, deterministic rules, and clear separation between canonical events and derived projections.

## Related Concepts

- [[EventStorePort]]
- [[EventLedger]]
- [[ReplaySession]]
- [[LifecycleEvent]]
- [[TF-0010 In-Memory Event Store]]
