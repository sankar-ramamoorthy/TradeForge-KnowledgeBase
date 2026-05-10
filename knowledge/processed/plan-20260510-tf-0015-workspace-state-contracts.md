---
title: TF-0015 Workspace State Contracts Planning Synthesis
type: processed-planning
status: processed
created: 2026-05-10
source:
  - knowledge/raw/20260510-tf-0015-workspace-state-contracts-plan.md
related:
  - TF-0015
  - Workspace
  - Derived State
  - Replay
  - ADR 0012
  - ADR 0013
---

# TF-0015 Workspace State Contracts Planning Synthesis

## Processing Result

TF-0015 should produce implementation-facing workspace state contracts, not projection logic and not new workspace doctrine.

The runtime contract catalog should align with the eight workspace routes already accepted in ADR 0012 and implemented by TF-0014. The older six-workspace acceptance wording should be clarified in runtime operational docs during issue synchronization.

## Stable Constraints

- Workspace state contracts describe derived read-model shape.
- Contracts must distinguish canonical, derived, inferred, and advisory fields.
- Contracts may identify canonical event inputs, but they do not make workspace state canonical.
- Contracts may declare allowed lifecycle-aware actions, but they do not execute actions.
- Contracts must preserve replay requirements explicitly.

## Runtime Boundary

This belongs in `src/services/workspace_engine/` as a service-layer contract boundary. It should not import infrastructure, app APIs, event stores, or lifecycle orchestration services.

Domain event and lifecycle types may be referenced only as stable identifiers if needed, but the preferred implementation is string-based input descriptors to keep the contract layer declarative and low-coupled.

## ADR Evaluation

No new ADR is required if implementation remains declarative and subordinate to ADR 0012 and ADR 0013.
