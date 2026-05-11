---
title: TF-0021 Workspace Projection Read Models Implementation Capture
type: raw-implementation-capture
status: raw
created: 2026-05-11
issue: TF-0021
milestone: M6
branch: feature/tf-0021-workspace-projection-read-models
source_runtime_repo: C:/Users/bosto/dockerstuff/TradeForge
---

# TF-0021 Workspace Projection Read Models Implementation Capture

## Implementation Summary

Implemented deterministic workspace projection read models for the ADR 0012 workspace set.

Runtime changes:

- Added `src/services/workspace_engine/projections.py`.
- Exported projection read-model classes from `src/services/workspace_engine/__init__.py`.
- Added `tests/test_workspace_projection_read_models.py`.
- Narrowed an existing routing boundary test so it continues to validate routing modules without blocking the new projection module from reading event history.

## Architecture Result

Workspace projections are immutable, derived, persona/workspace-scoped read models over ordered EventEnvelope history. They preserve source event references, contract field authority classes, source event types, lifecycle context derived from existing deterministic rules, and authority boundaries from workspace contracts.

The projection service reads from the EventStore port and does not append events. Projection projectors are compatible with the existing ProjectionRebuildPipeline protocol.

## Event And Lifecycle Impact

No new event types were introduced. No lifecycle transition semantics changed.

Lifecycle state is only derived from scoped event history for read-model context. Workspace projections do not approve, execute, complete, or mutate lifecycle state.

## Replay Impact

Projection output is deterministic for a given ordered event history and persona context. The read models preserve source event references and can be rebuilt through the projection rebuild pipeline.

## Validation Executed

- `uv run pytest tests\\test_workspace_projection_read_models.py` - passed
- `uv run pytest` - 128 passed
- `uv run ruff check .` - passed
- `uv run mypy src tests` - passed

## Follow-Up Boundaries

TF-0022 should implement operational attention queue semantics separately. TF-0023 should implement workspace summaries separately. TF-0021 intentionally does not add UI, API endpoints, persistence, AI summaries, or new event taxonomy.
