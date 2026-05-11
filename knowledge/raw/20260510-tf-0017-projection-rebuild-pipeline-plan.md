---
title: TF-0017 Projection Rebuild Pipeline Plan
type: raw
status: draft
created: 2026-05-10
tags:
  - TradeForge
  - runtime
  - TF-0017
  - M5
  - projection
  - replay
---

# TF-0017 Projection Rebuild Pipeline Plan

Runtime issue TF-0017 continues Milestone 5 replay and projection foundation work.

## Issue Discipline

- Issue ID: TF-0017
- Milestone: M5
- Title: Implement projection rebuild pipeline
- Runtime branch: `M5/tf-0017-projection-rebuild-pipeline`
- Affected layer: services
- Impacted ADRs: ADR 0001, ADR 0004, ADR 0008, ADR 0014
- Impacted invariants: Event Sourcing, Replay, Layer Separation

## Acceptance Criteria

- Projections can be rebuilt from event history.
- Rebuild order is deterministic.
- Rebuild output does not become canonical truth.
- Tests cover repeatable projection output.

## Runtime Design

Add a services-layer projection rebuild pipeline under `src/services/projections/`.

The pipeline reads ordered event history once from the `EventStore` port and passes that same history to configured projection rebuild targets.

Rebuild targets should be explicit and ordered.

The rebuild report should be immutable and marked as derived state.

The report should include:

- source event count
- source event type sequence
- rebuilt projection outputs
- projection names in rebuild order
- derived authority marker

## Boundaries

Out of scope:

- durable projection persistence
- append of `system.projection_rebuilt`
- background rebuild jobs
- replay timeline engine
- historical reconstruction pipeline
- API endpoints

## Event And Lifecycle Impact

No new event types are introduced.

No lifecycle transitions are created or validated.

Projection rebuild describes derived output from event history; it does not mutate canonical truth.

## ADR Checkpoint

No new ADR is required for TF-0017 because this issue implements accepted event-sourcing, workspace projection, and replay doctrine without introducing persistence authority, event semantics, lifecycle authority, or external integration boundaries.

## Validation Plan

- Service tests for rebuilding replay projection from event history.
- Service tests for deterministic target order.
- Service tests for repeatable report output.
- Service tests for derived authority and immutability.
- Service tests proving no append occurs during rebuild.
- Layer-boundary tests preventing services from depending on infrastructure or app layers.
- Run `uv run pytest`, `uv run ruff check .`, and `uv run mypy src tests`.
