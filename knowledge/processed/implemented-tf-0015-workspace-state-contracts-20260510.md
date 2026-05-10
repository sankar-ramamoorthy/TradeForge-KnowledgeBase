---
title: Implemented TF-0015 Workspace State Contracts
type: processed-implementation
status: processed
created: 2026-05-10
source:
  - knowledge/raw/20260510-implemented-tf-0015-workspace-state-contracts.md
related:
  - TF-0015
  - Workspace
  - Workspace State
  - Derived State
  - Replay
  - ADR 0012
---

# Implemented TF-0015 Workspace State Contracts

## Processing Result

TF-0015 stabilizes the runtime contract boundary for workspace state without introducing projection execution.

The implementation aligns the runtime with ADR 0012 by defining contracts for the same eight workspace identifiers used by TF-0014 routing. This resolves the mismatch between the older six-workspace issue text and the accepted ADR 0012 workspace set.

## Durable Runtime Learning

Workspace state contracts are declarative implementation guidance. They identify what future projections must provide, what source inputs are required, what replay must reconstruct, and where authority stops.

They are not:

- canonical state
- stored projections
- event semantics
- lifecycle transition logic
- UI routes or screens

## Preserved Invariants

- Workspace state remains derived and replayable.
- Canonical contract fields are limited to source references.
- Inferred and advisory fields remain distinguishable from event-backed facts.
- Lifecycle-aware actions remain boundary declarations only.
- No infrastructure or event-store dependency was introduced.

## Follow-Up

M5 replay/projector foundation should use these contracts as read-model targets while preserving projection discardability and replay determinism.
