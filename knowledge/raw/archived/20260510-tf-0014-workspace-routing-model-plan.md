---
title: TF-0014 Workspace Routing Model Plan
type: raw-planning-capture
status: raw
created: 2026-05-10
related:
  - TF-0014
  - ADR 0004
  - ADR 0007
  - ADR 0009
  - ADR 0012
---

# TF-0014 Workspace Routing Model Plan

## Issue Scope

Runtime issue TF-0014 creates the workspace routing model for Milestone 4.

The issue is limited to runtime route and entrypoint modeling for MVP workspaces. It must not implement frontend routing, workspace projections, persistent workspace state, or lifecycle mutation behavior.

## Semantic Alignment

Relevant KB doctrine:

- Workspaces are persona-scoped operational decision environments.
- Workspaces are not UI tabs, dashboards, or pages.
- UI/routes are windows into workspace cognition, not workspace truth.
- Persona context must be explicit.
- Derived state and canonical state must remain distinguishable.

## Runtime Design

Add immutable service/app routing contracts that define:

- bounded workspace route identifiers
- human-readable workspace route names
- path templates for runtime entrypoints
- persona/workflow context carried through route construction
- validation for unknown workspace identifiers

Routes must not depend on the event store, lifecycle services, infrastructure adapters, or UI frameworks.

## Event Impact

No new events are introduced.

Routing is not a fact in the canonical ledger unless future workspace open/close/context update events are explicitly introduced under a separate issue.

## Lifecycle Impact

No lifecycle stage changes are introduced.

Route construction must not approve, execute, or advance any decision lifecycle state.

## Replay Impact

The route model preserves persona and selected workflow context so future projections and replay surfaces can reconstruct what workspace context a user intended to enter. The route model itself is not replay truth.

## Testing Strategy

Tests should verify:

- the route catalog is bounded to the accepted workspace set
- route construction preserves persona and workflow context
- unknown workspace route IDs are rejected
- route modules do not depend on event-store, lifecycle, or infrastructure mutation paths

## ADR Checkpoint

No new ADR is required if implementation remains a lean route/entrypoint contract under ADR 0012. A new ADR would be required if routing introduced new workspace semantics, projection behavior, or lifecycle authority.
