---
title: TF-0030 Workspace Projection API Implementation Capture
type: raw-development-capture
status: raw
created: 2026-05-11
issue: TF-0030
milestone: M7
---

# TF-0030 Workspace Projection API Implementation Capture

## Implementation Summary

Implemented read-only FastAPI endpoints for workspace projection read models:

- `GET /workspaces`
- `GET /workspaces/{route_id}`

The endpoints require explicit persona/workspace context and delegate projection
construction to `WorkspaceProjectionReadService`.

## Runtime Changes

- Added app factory wiring for `WorkspaceProjectionReadService`.
- Added workspace projection response models in the FastAPI route layer.
- Allowed workspace projection services to accept explicit
  `WorkspaceProjectionContext` without fabricating a full persona profile at the
  HTTP boundary.
- Translated unknown workspace route IDs into explicit 404 HTTP responses.
- Added FastAPI tests for derived authority, persona/workspace scoping,
  source-event links, unknown-route handling, required context, and read-only
  behavior.

## Replay And Event Integrity

No new events were introduced.

Workspace API reads do not append events. Projection output preserves source
event references, source event counts, source event types, field authority, and
authority boundaries.

## Lifecycle Integrity

No lifecycle mutation endpoints were added.

Lifecycle state in workspace responses is derived from event history. Lifecycle
authority remains with the existing lifecycle orchestration service.

## Verification

- `uv run pytest tests\test_fastapi_runtime.py tests\test_workspace_projection_read_models.py`
- `uv run pytest`
- `uv run ruff check .`
- `uv run mypy src tests`

## Follow-Up

No canonical KB doctrine update appears necessary. The implementation confirms
the existing ADR boundary: workspace APIs are read adapters over derived
projections, not new state owners.
