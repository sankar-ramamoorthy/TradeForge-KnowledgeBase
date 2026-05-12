---
title: TF-0032 Workspace Routing System Implementation
type: raw
status: raw
created: 2026-05-12
issue: TF-0032
milestone: M7
tags:
  - TradeForge
  - runtime-kb-loop
  - frontend
  - workspace-routing
---

# TF-0032 Workspace Routing System Implementation

## Runtime Outcome

Implemented TF-0032 on runtime branch `feature/tf-0032-workspace-routing-system`.

Runtime changes:

- Added `frontend/src/workspaceRouting.ts` with typed route metadata for the six core MVP workspaces.
- Updated `frontend/src/App.tsx` to support browser-history routing under `/workspaces/...`.
- Preserved `persona_id`, `selected_workflow_id`, and `decision_id` query context across workspace navigation.
- Updated `frontend/src/styles.css` for the route navigation, context rail, and route-specific derived workspace surfaces.`r`n- Updated `frontend/vite.config.ts` so HTML navigation to `/workspaces/...` falls through to the SPA while API-style workspace requests remain proxied to FastAPI.
- Updated runtime operational state in `DOCS/ISSUE_REGISTER.md` and `DOCS/Milestone_Roadmap_v2.md`.

## Semantic Boundary

The implementation treats routes as frontend entrypoints only. Routes do not own workspace state, lifecycle transitions, event truth, replay reconstruction, or persona interpretation.

## Event Impact

No events were added or changed.

## Lifecycle Impact

No lifecycle authority was added. Any future lifecycle action still must route through runtime lifecycle APIs.

## Replay Impact

Route URLs preserve explicit context parameters, making workspace navigation more reconstructable without making browser state replay authority.

## Validation

Completed validation:

- `npm.cmd run typecheck`
- `npm.cmd run lint`
- `npm.cmd run build` after sandbox escalation for Vite/esbuild access
- `uv run pytest tests\\test_workspace_routing.py`
- `uv run pytest`

## Follow-Up Boundary

TF-0033 remains responsible for shared operational layout primitives. TF-0034 remains responsible for authentication/session semantics. M8 workspace issues remain responsible for full workspace implementation.

