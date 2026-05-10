---
title: TF-0016 Replay Projector Foundation
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
  - knowledge/raw/archived/20260510-tf-0016-replay-projector-foundation-plan.md
related:
  - "[[Runtime KB Development Loop]]"
  - "[[Replay System]]"
  - "[[Event Ledger]]"
  - "[[Projection]]"
---

# TF-0016 Replay Projector Foundation

TF-0016 begins the Milestone 5 runtime implementation of replay and projection foundations.

## Stabilized Planning Summary

The replay projector foundation should remain a deterministic reconstruction component.

It consumes ordered event history and emits discardable derived state.

It does not create events, persist projections, call live APIs, query UI state, invoke AI, or own lifecycle authority.

## Runtime Boundary

Runtime implementation should split responsibility as follows:

- domain replay projector: pure deterministic projection from `EventEnvelope` history
- service replay projection wrapper: orchestration through the `EventStore` port

This preserves the Event Ledger as canonical truth while allowing projection output to be rebuilt or discarded.

## Lifecycle Interpretation

TF-0016 reconstructs current decision lifecycle state from existing lifecycle event mappings.

Replay does not advance lifecycle state.

Replay does not validate requested transitions.

Replay only describes what the ordered event history derives.

## ADR Interpretation

No new ADR is required for TF-0016.

The issue implements accepted replay doctrine from ADR 0008 and ADR 0014 and does not introduce a new architectural decision.

## Deferred Work

The following remain deferred to later M5 issues:

- TF-0017 projection rebuild pipeline
- TF-0018 replay timeline engine
- TF-0019 historical reconstruction pipeline

## Invariant Alignment

The design preserves:

- Event Sourcing: all reconstructed state derives from event history
- Replay: no live APIs or mutable projections are used as truth
- Event Integrity: no event mutation or new event semantics are introduced
- Layer Separation: domain remains pure; services orchestrate through ports

## KB Processing Result

This note is a processed planning synthesis, not canonical doctrine by itself.

Stable knowledge promoted from this note:

- replay projection is deterministic derived state
- replay projection is descriptive, not lifecycle-authoritative
- replay projection reads ordered Event Ledger history and produces discardable output
- service orchestration may wrap projection through the Event Store port without changing the domain boundary

Ontology impact:

- [[Projection]] is stable enough to define as a KB entity because runtime ADRs and M5 planning now depend on the term.
- `ReplayProjector` remains implementation vocabulary and should not be promoted as a canonical entity unless later issues reveal a durable semantic boundary.

Workflow impact:

- TF-0016 starts M5 replay/projection work after M4 workspace contracts.
- TF-0017 through TF-0019 should extend projection and replay without changing the TF-0016 authority boundary.

ADR impact:

- no new ADR is required because TF-0016 implements ADR 0001, ADR 0003, ADR 0008, and ADR 0014.

Operational synchronization:

- source raw note has been archived, not deleted from history.
- KB issue and roadmap indexes should show M4 as Done, M5 as In Progress, and TF-0016 as Done.
- older processed notes may reference replay projector work under the previous TF-0014 numbering; Roadmap v2 supersedes that mapping and identifies TF-0016 as the replay projector foundation issue.
