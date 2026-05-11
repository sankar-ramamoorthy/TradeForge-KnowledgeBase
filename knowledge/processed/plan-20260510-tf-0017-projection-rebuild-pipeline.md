---
title: TF-0017 Projection Rebuild Pipeline
type: processed
status: active
created: 2026-05-10
tags:
  - TradeForge
  - runtime
  - TF-0017
  - M5
  - projection
  - replay
source:
  - knowledge/raw/archived/20260510-tf-0017-projection-rebuild-pipeline-plan.md
related:
  - "[[Runtime KB Development Loop]]"
  - "[[Projection]]"
  - "[[Replay System]]"
  - "[[Event Ledger]]"
---

# TF-0017 Projection Rebuild Pipeline

TF-0017 adds the runtime service flow for rebuilding derived projections from event history.

## Stabilized Planning Summary

Projection rebuild should be deterministic orchestration over existing projection rules.

It must not create canonical truth.

It must not persist projection state.

It must not append rebuild events in this issue.

## Runtime Boundary

The service layer owns rebuild orchestration.

The event store port supplies ordered canonical event history.

Projection targets consume event history and return derived outputs.

The rebuild report remains a discardable derived artifact.

## Determinism

Determinism depends on:

- reading event history once
- preserving configured target order
- rejecting duplicate projection target names
- passing the same ordered event tuple to each target

## Deferred Work

The following remain deferred:

- durable projection persistence
- system events for rebuild observability
- replay timeline engine
- historical reconstruction pipeline
- API exposure

## Invariant Alignment

The design preserves:

- Event Sourcing: rebuild inputs come from Event Ledger history
- Replay: projections are reconstructed from deterministic rules
- Layer Separation: services orchestrate; infrastructure remains behind ports
- Derived State Distinction: rebuild reports are non-authoritative

## KB Processing Result

This note is a processed planning synthesis, not canonical doctrine by itself.

Stable knowledge promoted from this note:

- projection rebuild is deterministic services-layer orchestration over ordered Event Ledger history.
- projection rebuild reports are derived artifacts and do not become canonical truth.
- projection rebuild must preserve explicit target ordering and reject duplicate target names.
- rebuild orchestration may use the Event Store port without changing Event Ledger authority.

Ontology impact:

- [[Projection]] remains the stable canonical concept.
- `ProjectionRebuildPipeline`, `ProjectionRebuildTarget`, and rebuild report/result names remain runtime implementation vocabulary unless later issues reveal durable semantic boundaries.

Workflow impact:

- TF-0017 extends TF-0016 by making deterministic projection rebuild orchestration explicit.
- TF-0018 and TF-0019 should consume this rebuild boundary without introducing projection authority, persistence-as-truth, or lifecycle mutation.

ADR impact:

- no new ADR is required because TF-0017 implements existing event-sourcing, workspace projection, replay, and replay-centric UX doctrine from ADR 0001, ADR 0004, ADR 0008, and ADR 0014.

Operational synchronization:

- source raw note has been archived, not deleted from history.
- KB issue and roadmap indexes should show TF-0017 as Done while M5 remains In Progress.
