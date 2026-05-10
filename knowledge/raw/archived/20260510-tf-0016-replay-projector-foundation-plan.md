---
title: TF-0016 Replay Projector Foundation Plan
type: raw
status: draft
created: 2026-05-10
tags:
  - TradeForge
  - runtime
  - TF-0016
  - M5
  - replay
  - projection
---

# TF-0016 Replay Projector Foundation Plan

Runtime issue TF-0016 starts Milestone 5 replay and projection foundation work.

## Issue Discipline

- Issue ID: TF-0016
- Milestone: M5
- Title: Implement replay projector foundation
- Runtime branch: `M5/tf-0016-replay-projector-foundation`
- Affected layer: domain, services
- Impacted ADRs: ADR 0001, ADR 0003, ADR 0008, ADR 0014
- Impacted invariants: Event Sourcing, Replay, Event Integrity, Layer Separation

## Acceptance Criteria

- Replay consumes event history, not live APIs or UI state.
- Projector output is deterministic for the same event stream.
- Replay reconstructs lifecycle state for a basic workflow.
- Projection output remains derived and discardable.

## Runtime Design

Add a pure domain replay projector under `src/domain/replay/`.

The projector consumes ordered `EventEnvelope` history and returns an immutable `ReplayProjection`.

`ReplayProjection` should expose only derived reconstruction metadata:

- projection authority marker
- source event count
- source event type sequence
- last event timestamp
- reconstructed lifecycle state

Add a services-layer `ReplayProjectionService` that reads from the `EventStore` port and delegates projection to the domain projector.

## Boundaries

Out of scope:

- projection persistence
- replay timeline engine
- historical market reconstruction
- replay API endpoints
- AI replay narration
- frontend replay workspace

## Event And Lifecycle Impact

No new event types are introduced.

No lifecycle transition is created by replay.

Lifecycle state is reconstructed from existing lifecycle event mapping and remains derived.

## ADR Checkpoint

No new ADR is required for TF-0016 because this issue implements accepted replay doctrine from ADR 0008 and ADR 0014 without introducing new replay semantics, storage authority, lifecycle authority, or integration boundary.

## Validation Plan

- Domain tests for deterministic replay projection.
- Domain tests for lifecycle reconstruction from a basic workflow.
- Domain tests for empty event history and immutable projection output.
- Service tests proving projection consumes `EventStore` history through the port.
- Layer-boundary tests preventing domain/service dependency on infrastructure or app layers.
- Run `uv run pytest`, `uv run ruff check .`, and `uv run mypy src tests`.
