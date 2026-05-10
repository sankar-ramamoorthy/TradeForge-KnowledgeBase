---
title: Implemented TF-0014 Workspace Routing Model
type: raw-implementation-capture
status: raw
created: 2026-05-10
related:
  - TF-0014
  - ADR 0012
---

# Implemented TF-0014 Workspace Routing Model

## Implementation Summary

Runtime now includes a bounded workspace routing model for Milestone 4.

Added service-layer contracts under `src/services/workspace_engine/`:

- `WorkspaceRouteId`
- `WorkspaceRouteDefinition`
- `WorkspaceRouteContext`
- `WorkspaceRoute`
- `WorkspaceRouter`
- `UnknownWorkspaceRouteError`

Added app-layer entrypoint exposure under `src/app/workspace_routes.py`.

The route catalog covers the accepted ADR 0012 workspace set:

- Operating Workspace
- Opportunity Workspace
- Plan Review Workspace
- Active Position Workspace
- Replay Workspace
- Review Workspace
- Market Context Workspace
- Playbooks / Doctrine Workspace

## Boundary Preserved

Routes are runtime entrypoints only.

They do not:

- append events
- mutate lifecycle state
- call infrastructure adapters
- define workspace projections
- own canonical workspace truth
- implement frontend navigation

Persona context is required for route construction. Selected workflow and decision context can be carried through the route as context, but not as canonical lifecycle state.

## Verification

Runtime validation completed:

- `uv run pytest`
- `uv run ruff check .`
- `uv run mypy src tests`

## Operational Sync

Runtime issue state was updated:

- `DOCS/ISSUE_REGISTER.md` marks TF-0014 as Done.
- `DOCS/Milestone_Roadmap_v2.md` marks TF-0014 as Done within M4.

## Follow-Up Boundary

TF-0015 should define workspace state contracts. It should consume the routing boundary without allowing routes to become workspace truth or projection ownership.
