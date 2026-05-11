---
title: TF-0019 Historical Reconstruction Pipeline Plan
type: raw
status: draft
created: 2026-05-10
tags:
  - TradeForge
  - runtime
  - TF-0019
  - M5
  - replay
  - historical-reconstruction
---

# TF-0019 Historical Reconstruction Pipeline Plan

Runtime issue TF-0019 completes the Milestone 5 replay and projection foundation set.

## Issue Discipline

- Issue ID: TF-0019
- Milestone: M5
- Title: Implement historical reconstruction pipeline
- Runtime branch: `M5/tf-0019-historical-reconstruction-pipeline`
- Affected layer: services
- Impacted ADRs: ADR 0008, ADR 0014
- Impacted invariants: Replay, Historical Integrity, Layer Separation

## Acceptance Criteria

- Reconstruction can answer what was known and visible at replay time.
- Reconstruction does not call live APIs.
- Reconstruction keeps facts, derived state, and inferred state distinguishable.

## Runtime Design

Add a services-layer reconstruction pipeline under `src/services/replay/reconstruction.py`.

The pipeline reads ordered event history once through the `EventStore` port and composes existing deterministic replay pieces:

- `ReplayProjector` for lifecycle-derived state
- `ReplayTimelineBuilder` for visible timeline reconstruction

The pipeline should return immutable `HistoricalReconstruction` output with explicit buckets:

- canonical source facts
- derived reconstruction state
- inferred reconstruction state
- source-linked notes
- source-linked review artifacts

For TF-0019, inferred state should be represented as an explicit empty bucket because no inference engine exists yet.

## Boundaries

Out of scope:

- AI replay narration
- live market or broker API calls
- reconstruction persistence
- frontend/API exposure
- new event types
- lifecycle mutation
- inferred-state generation

## Event And Lifecycle Impact

No new event types are introduced.

No events are appended by historical reconstruction.

No lifecycle transitions are created or validated.

Lifecycle state is reconstructed through existing deterministic lifecycle projection.

## ADR Checkpoint

No new ADR is required for TF-0019 because the issue composes accepted replay projection and timeline behavior from ADR 0008 and ADR 0014 without adding persistence authority, event semantics, lifecycle authority, AI authority, or API/UI boundaries.

## Validation Plan

- Service tests for reconstruction from event history.
- Service tests proving facts, derived state, and inferred state are distinct.
- Service tests proving notes and review artifacts remain source-linked.
- Service tests proving no append occurs during reconstruction.
- Repeatability tests for the same event stream.
- Layer-boundary tests preventing services from depending on infrastructure or app layers.
- Run `uv run pytest`, `uv run ruff check .`, and `uv run mypy src tests`.
