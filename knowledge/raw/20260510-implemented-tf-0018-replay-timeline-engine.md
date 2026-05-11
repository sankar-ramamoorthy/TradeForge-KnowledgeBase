---
title: Implemented TF-0018 Replay Timeline Engine
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

# Implemented TF-0018 Replay Timeline Engine

Runtime issue TF-0018 was implemented on branch `M5/tf-0018-replay-timeline-engine`.

## Runtime Files Added

- `src/domain/replay/timeline.py`
- `src/services/replay/timeline.py`
- `tests/test_replay_timeline.py`
- `tests/test_replay_timeline_service.py`

## Runtime Files Updated

- `src/domain/replay/__init__.py`
- `src/services/replay/__init__.py`
- `DOCS/ISSUE_REGISTER.md`
- `DOCS/Milestone_Roadmap_v2.md`

## Implementation Summary

The runtime now has a deterministic replay timeline engine.

The domain builder consumes event history and emits immutable derived timeline entries.

Timeline entries include:

- source sequence
- entry kind
- event type
- event domain
- timestamp
- persona and workspace context
- entity references
- payload
- provenance
- derived lifecycle stage when applicable

The service wrapper reads history through the `EventStore` port and delegates to the domain builder.

## Replay And Historical Integrity

Timeline ordering is deterministic by event timestamp and source sequence.

Timeline entries preserve event references, payload, and provenance.

Lifecycle stages are derived from existing lifecycle event mappings.

Replay timeline construction does not depend on UI state, live APIs, AI output, or mutable projections.

## Architecture Boundary

No new event types were introduced.

No events are appended by timeline construction.

No lifecycle state is advanced or validated by the timeline engine.

No frontend timeline or API endpoint was introduced.

## Deferred Work

- interactive frontend timeline
- persisted timelines
- historical reconstruction pipeline
- AI replay narration
- replay API endpoints

## Validation Completed

- `uv run pytest` passed with 105 tests.
- `uv run ruff check .` passed.
- `uv run mypy src tests` passed.
