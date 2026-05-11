---
title: TF-0018 Replay Timeline Engine Plan
type: raw
status: draft
created: 2026-05-10
tags:
  - TradeForge
  - runtime
  - TF-0018
  - M5
  - replay
  - timeline
---

# TF-0018 Replay Timeline Engine Plan

Runtime issue TF-0018 continues Milestone 5 replay and projection foundation work.

## Issue Discipline

- Issue ID: TF-0018
- Milestone: M5
- Title: Implement replay timeline engine
- Runtime branch: `M5/tf-0018-replay-timeline-engine`
- Affected layer: domain, services
- Impacted ADRs: ADR 0008, ADR 0014
- Impacted invariants: Replay, Event Integrity, Historical Integrity

## Acceptance Criteria

- Timeline orders lifecycle, execution, review, and relevant system events deterministically.
- Timeline entries preserve event references and provenance.
- Timeline model supports replay UI without depending on UI state.

## Runtime Design

Add a pure domain timeline model under `src/domain/replay/timeline.py`.

The timeline builder consumes ordered event history and emits immutable derived `ReplayTimeline` output.

For TF-0018, replay-relevant entries include:

- lifecycle decision events
- execution events
- review events
- system events

Timeline ordering should be deterministic by:

```text
event timestamp
original event-stream sequence
```

Each timeline entry should preserve:

- event type
- timestamp
- event domain
- original source sequence
- persona and workspace context
- entity references
- payload
- provenance
- derived lifecycle stage when the event is a lifecycle event

Add a services-layer timeline service that reads through the `EventStore` port and delegates to the domain builder.

## Boundaries

Out of scope:

- interactive frontend timeline
- timeline persistence
- historical reconstruction pipeline
- AI replay narration
- live API calls
- new event types

## Event And Lifecycle Impact

No new event types are introduced.

No events are appended by the timeline engine.

No lifecycle transitions are created or validated.

Lifecycle stage fields are derived from the existing lifecycle event mapping only.

## ADR Checkpoint

No new ADR is required for TF-0018 because this issue implements accepted replay timeline doctrine from ADR 0008 and ADR 0014 without introducing persistence authority, event semantics, lifecycle authority, or UI/API boundaries.

## Validation Plan

- Domain tests for deterministic timeline ordering.
- Domain tests for filtering replay-relevant domains.
- Domain tests for preserving entity references, payload, and provenance.
- Domain tests for lifecycle stage derivation.
- Service tests proving event history is consumed through the `EventStore` port.
- Layer-boundary tests preventing domain/service dependency on infrastructure or app layers.
- Run `uv run pytest`, `uv run ruff check .`, and `uv run mypy src tests`.
