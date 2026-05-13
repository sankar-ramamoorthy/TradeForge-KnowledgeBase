---
title: TF-0030 Workspace Projection API Planning
type: raw-development-capture
status: raw
created: 2026-05-11
issue: TF-0030
milestone: M7
---

# TF-0030 Workspace Projection API Planning

## Issue Context

TF-0030 adds read-only workspace projection APIs to the TradeForge runtime.

The issue is scoped to exposing existing workspace projection read models through
the FastAPI application boundary. It does not add frontend rendering, projection
persistence, lifecycle mutation, authentication, or new event semantics.

## Semantic Boundary

Workspace projections remain derived state. The API is only an HTTP adapter over
the existing workspace projection read service.

The API must preserve:

- explicit persona and workspace context
- derived projection authority
- source event references
- field-level authority labels
- lifecycle state as event-derived state only
- authority boundaries that prevent workspace surfaces from becoming lifecycle
  or execution authority

## Architecture Interpretation

Affected runtime layers:

- app: FastAPI request/response models and route handlers
- services: existing `WorkspaceProjectionReadService` dependency wiring only

Unaffected layers:

- domain: no new domain semantics
- infrastructure: no new persistence adapter
- frontend: explicitly out of scope

## Event Impact

No new event types are required.

The API reads event history through the workspace projection read service. It
must not append events and must not expose projections as canonical truth.

## Lifecycle Impact

No lifecycle transitions are introduced.

Returned lifecycle state is derived from event history. The Decision Lifecycle
Engine remains the only lifecycle authority.

## Replay Impact

API responses should preserve enough source linkage for replay-compatible UI
surfaces:

- projection source event count and event types
- source event references
- projection field source events
- authority boundaries

## ADR Checkpoint

No new ADR is required because ADR 0004, ADR 0012, and ADR 0020 already define
the architectural decision:

- workspaces are derived projections
- workspace routes/screens do not own canonical state
- FastAPI is an HTTP boundary and delegates domain behavior to services

## Implementation Plan

1. Wire `WorkspaceProjectionReadService` into the FastAPI app factory using the
   shared event store.
2. Add response models for workspace projection context, field, lifecycle state,
   source event references, individual projection, and projection set.
3. Add GET `/workspaces` for all registered workspace projections.
4. Add GET `/workspaces/{route_id}` for one workspace projection.
5. Require explicit query parameters for `persona_id`, `persona_version`, and
   `workspace_id`; accept optional `workflow_id` and `decision_id`.
6. Translate unknown route IDs into explicit 404 responses without leaking
   service exceptions.
7. Extend FastAPI tests for read-only behavior, derived authority, context
   scoping, unknown-route handling, and removal of the prior future-endpoint
   assertion.

## Validation Targets

- Workspace invariant preserved: APIs expose cognitive projection state, not UI
  dashboards or stored workspace truth.
- Event sourcing invariant preserved: no append occurs during reads.
- Lifecycle invariant preserved: no endpoint bypasses lifecycle services.
- Persona invariant preserved: context is explicit in every projection request.
- Layer separation preserved: route handlers delegate projection construction to
  the service layer.
