---
title: Implemented TF-0018 Replay Timeline Engine
type: processed
status: active
created: 2026-05-10
tags:
  - TradeForge
  - runtime
  - TF-0018
  - M5
  - replay
  - timeline
source:
  - knowledge/raw/archived/20260510-implemented-tf-0018-replay-timeline-engine.md
related:
  - "[[Runtime KB Development Loop]]"
  - "[[Replay System]]"
  - "[[ReplayTimeline]]"
  - "[[Event Ledger]]"
---

# Implemented TF-0018 Replay Timeline Engine

TF-0018 established the runtime replay timeline model for Milestone 5.

## Stabilized Implementation Knowledge

Replay timeline construction is a derived reconstruction process.

It consumes event history and produces immutable entries that preserve source event context.

It does not create canonical state, append events, persist timelines, call live APIs, use AI output, or depend on UI state.

## Runtime Architecture Result

The implementation introduced:

- `ReplayTimelineBuilder`
- `ReplayTimeline`
- `ReplayTimelineEntry`
- `ReplayTimelineEntryKind`
- `ReplayTimelineService`

The domain layer owns deterministic timeline construction.

The service layer reads through the `EventStore` port and delegates to the domain builder.

## Determinism Result

Timeline entries are ordered by:

```text
event timestamp
source event sequence
```

This preserves deterministic output even when multiple events share a timestamp.

## Historical Integrity Result

Timeline entries preserve:

- source sequence
- event type and domain
- timestamp
- persona and workspace context
- entity references
- payload
- provenance
- derived lifecycle stage where applicable

This supports replay and review surfaces without turning UI state into truth.

## Operational State

Runtime issue TF-0018 is marked Done in:

- `DOCS/ISSUE_REGISTER.md`
- `DOCS/Milestone_Roadmap_v2.md`

## Validation

Runtime validation completed:

- `uv run pytest`
- `uv run ruff check .`
- `uv run mypy src tests`

## Deferred Boundaries

Interactive frontend timeline, persisted timeline storage, historical reconstruction composition, AI narration, and API exposure remain deferred to later issues.

## KB Processing Result

This note is a processed implementation synthesis.

Stable knowledge promoted from this note:

- replay timeline is now implemented as deterministic derived reconstruction over ordered event history.
- timeline output is immutable and preserves source context needed for replay and review.
- timeline ordering depends on event timestamp plus source event sequence.
- replay timeline does not append events, persist authoritative state, or create lifecycle authority.

Ontology impact:

- [[ReplayTimeline]] is the stable KB concept for this capability.
- `ReplayTimelineBuilder`, `ReplayTimeline`, `ReplayTimelineEntry`, `ReplayTimelineEntryKind`, and `ReplayTimelineService` remain runtime implementation vocabulary.
- no new event, lifecycle, AI, or workspace authority concept was introduced.

Workflow and playbook impact:

- TF-0018 confirms that M5 should keep replay timeline generation separate from historical reconstruction composition, API exposure, UI replay workspace behavior, and AI narration.
- no playbook change is required.

ADR impact:

- no new ADR is required.
- accepted replay doctrine from ADR 0008 and replay-centric UX direction from ADR 0014 remain sufficient.

Operational synchronization:

- runtime issue TF-0018 is Done.
- runtime roadmap keeps M5 In Progress with TF-0019 still pending.
- source raw note has been archived, preserving traceability while avoiding raw-note accumulation.
