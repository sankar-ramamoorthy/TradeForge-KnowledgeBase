---
title: TF-0014 Workspace Routing Model Planning Synthesis
type: processed-planning
status: processed
created: 2026-05-10
source:
  - knowledge/raw/20260510-tf-0014-workspace-routing-model-plan.md
related:
  - TF-0014
  - Workspace
  - Persona
  - ADR 0012
---

# TF-0014 Workspace Routing Model Planning Synthesis

## Processing Result

TF-0014 should be implemented as a lean runtime entrypoint contract, not as workspace doctrine.

The accepted architecture already exists in ADR 0012 and canonical KB workspace doctrine. The runtime task translates those constraints into code-level route boundaries so later app/frontend work does not accidentally treat React routes, pages, or navigation entries as workspace truth.

## Stable Constraints

- Workspace routes are entrypoints only.
- The route catalog is bounded to the accepted workspace set.
- Persona context must be explicit in every constructed route.
- Selected workflow context may travel with a route, but it remains context rather than canonical lifecycle state.
- Route construction must not append events, mutate lifecycle state, call infrastructure adapters, or own projection state.

## Runtime Boundary

This work belongs in runtime services/app layers as routing contracts. It does not belong in domain event semantics because no new canonical fact is introduced.

Future issues may add event-backed workspace context updates or projection read models, but those remain outside TF-0014.

## ADR Evaluation

No new ADR is required for TF-0014 if the implementation remains within ADR 0012. Introducing new workspace semantics, persisted route state, or route-driven lifecycle behavior would require a separate ADR checkpoint.
