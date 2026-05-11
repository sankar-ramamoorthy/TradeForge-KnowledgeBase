---
title: TF-0030 Workspace Projection API Synthesis
type: processed-development-synthesis
status: processed
created: 2026-05-11
source: knowledge/raw/20260511-tf-0030-workspace-projection-api-planning.md
issue: TF-0030
milestone: M7
---

# TF-0030 Workspace Projection API Synthesis

## Stabilized Interpretation

Workspace projection APIs are HTTP read boundaries over existing derived
workspace projection services. They do not introduce new workspace semantics.

The durable architectural interpretation is:

- the Event Ledger remains canonical truth
- workspace projections remain derived and discardable
- persona and workspace context must be explicit at the API boundary
- HTTP handlers translate requests and responses but do not define projection
  rules
- lifecycle state exposed by projection APIs is derived state, not lifecycle
  authority

## Doctrine Impact

No canonical KB doctrine update is required for TF-0030.

Existing doctrine is sufficient:

- `WORKSPACES.md` defines workspaces as persona-scoped operational
  environments, not UI containers
- `INVARIANTS.md` distinguishes canonical and derived state
- `EVENT_TAXONOMY.md` requires event-backed truth and replayable derivation
- runtime ADR 0004 defines workspace projections
- runtime ADR 0012 defines workspace architecture
- runtime ADR 0020 defines FastAPI as an adapter boundary

## Implementation Constraint

The runtime implementation should expose source-linked projection read models
without creating hidden mutable workspace state or giving workspace APIs
authority to mutate lifecycle state.

This issue is an adapter-layer implementation, not a semantic expansion.
