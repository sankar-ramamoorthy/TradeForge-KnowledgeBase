---
title: Implemented TF-0016 Replay Projector Foundation
type: processed
status: active
created: 2026-05-10
tags:
  - TradeForge
  - runtime
  - TF-0016
  - M5
  - replay
  - projection
source:
  - knowledge/raw/archived/20260510-implemented-tf-0016-replay-projector-foundation.md
related:
  - "[[Runtime KB Development Loop]]"
  - "[[Replay System]]"
  - "[[Event Ledger]]"
  - "[[Projection]]"
---

# Implemented TF-0016 Replay Projector Foundation

TF-0016 established the first runtime replay projection foundation for Milestone 5.

## Stabilized Implementation Knowledge

Replay projection is implemented as deterministic reconstruction from ordered event history.

Projection output is immutable, derived, and discardable.

The implementation preserves the Event Ledger as canonical truth because it reads event history and produces no events or persisted projection state.

## Runtime Architecture Result

The implementation introduced:

- domain replay projector under `src/domain/replay/`
- services-layer projection wrapper under `src/services/replay/`
- tests for deterministic projection, lifecycle reconstruction, empty history, immutability, service orchestration, and layer boundaries

The domain layer remains pure and framework-free.

The service layer orchestrates only through the `EventStore` port.

## Lifecycle Result

Replay projection reconstructs the current lifecycle state for a basic workflow through the existing lifecycle derivation rules.

This keeps replay descriptive rather than authoritative.

Replay projection does not validate requested transitions, append lifecycle events, or modify workflow state.

## Operational State

Runtime issue TF-0016 is marked Done in:

- `DOCS/ISSUE_REGISTER.md`
- `DOCS/Milestone_Roadmap_v2.md`

Milestone 5 is now In Progress.

## Validation

Runtime validation completed:

- `uv run pytest`
- `uv run ruff check .`
- `uv run mypy src tests`

## Deferred Boundaries

TF-0016 intentionally does not implement projection persistence, replay timelines, historical reconstruction, replay APIs, AI narration, or frontend replay workspace behavior.

Those remain bounded to later M5/M7/M8 issues.

## KB Processing Result

This note is a processed implementation synthesis.

Stable knowledge promoted from this note:

- replay projection is now implemented as a pure domain reconstruction component plus a services-layer wrapper.
- projection output is immutable, derived, and discardable.
- replay projection derives lifecycle state but does not validate, approve, advance, or persist lifecycle transitions.
- the service boundary depends on the Event Store port and does not bypass the Event Ledger.

Ontology impact:

- [[Projection]] should be treated as a stable derived-state concept.
- `ReplayProjector` should remain runtime implementation vocabulary for now.
- No new lifecycle, event, AI, or workspace authority concept was introduced.

Workflow and playbook impact:

- TF-0016 confirms that M5 work should proceed from deterministic replay foundations before timelines, historical reconstruction, APIs, or UI surfaces.
- No playbook change is required.

ADR impact:

- no new ADR is required.
- accepted replay and projection doctrine remains sufficient.

Operational synchronization:

- runtime issue TF-0016 is Done.
- runtime roadmap marks M4 Done and M5 In Progress.
- source raw note has been archived, preserving traceability while avoiding raw-note accumulation.
- older processed notes may retain historical references to replay projector work as TF-0014; current runtime authority assigns replay projector foundation to TF-0016.
