---
title: TF-0015 Workspace State Contracts Planning Synthesis
type: processed-planning
status: processed
created: 2026-05-10
source:
  - knowledge/raw/20260510-tf-0015-workspace-state-contracts-plan.md
archived_source:
  - knowledge/raw/archived/20260510-tf-0015-workspace-state-contracts-plan.md
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

## Ontology Implications

No new canonical entity should be promoted yet.

`Workspace State Contract` is a strong candidate concept, but it is still implementation-facing until M5 proves how replay/projector infrastructure consumes it. For now it should remain a processed runtime learning rather than a glossary or entity addition.

The important semantic distinction is stable:

- `Workspace` is a persona-scoped operational cognition environment.
- `Workspace Route` is an entrypoint.
- `Workspace State Contract` is a derived read-model contract.
- Canonical truth remains in event-backed lifecycle and ledger facts.

## Workflow And Playbook Implications

TF-0015 completes the lean M4 runtime shape when paired with TF-0014:

- routes define where workspace cognition is entered
- contracts define what derived state a workspace expects
- neither routes nor contracts own truth

No new playbook is required, but M5 planning should explicitly consume TF-0015 contracts as projector targets.

## Operational Synchronization

The runtime issue register marks TF-0015 done.

Because TF-0014 and TF-0015 are both done, M4 should be considered complete once the runtime roadmap milestone status is updated from Planned to Done.
