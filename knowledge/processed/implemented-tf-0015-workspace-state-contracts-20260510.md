---
title: Implemented TF-0015 Workspace State Contracts
type: processed-implementation
status: processed
created: 2026-05-10
source:
  - knowledge/raw/20260510-implemented-tf-0015-workspace-state-contracts.md
archived_source:
  - knowledge/raw/archived/20260510-implemented-tf-0015-workspace-state-contracts.md
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

## Ontology Implications

The implementation strengthens, but does not yet canonize, `Workspace State Contract` as a semantic object.

Promotion to `knowledge/entities/` should wait until M5 demonstrates the contract's replay/projector role. Premature promotion would risk locking an implementation boundary before the projection model validates it.

## Workflow And Playbook Implications

M4 runtime architecture is now complete:

- TF-0014: workspace entrypoint/routing boundary
- TF-0015: workspace state contract boundary

The next disciplined step is TF-0016. M5 should not add Postgres, FastAPI, React, AI, market context automation, or scenario discovery.

## ADR Implications

No new ADR is required.

ADR 0012 covers workspace architecture and ADR 0013 covers derived operational attention. TF-0015 stayed within those decisions by remaining declarative and non-authoritative.

## Operational Synchronization

Runtime issue state is synchronized for TF-0015.

The only drift identified during KB processing was milestone-level status: Roadmap v2 still labeled M4 as Planned even though both linked runtime issues are Done. That should be corrected to mark M4 Done.
