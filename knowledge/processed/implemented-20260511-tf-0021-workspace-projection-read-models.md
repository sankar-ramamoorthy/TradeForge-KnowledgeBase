---
title: Implemented - TF-0021 Workspace Projection Read Models
type: processed-implementation
status: processed
created: 2026-05-11
source: knowledge/raw/archived/20260511-implemented-tf-0021-workspace-projection-read-models.md
issue: TF-0021
milestone: M6
branch: feature/tf-0021-workspace-projection-read-models
related:
  - [[Projection]]
  - [[Persona Workspace]]
  - [[Replay System]]
  - [[Derived State]]
---

# Implemented - TF-0021 Workspace Projection Read Models

## Stable Implementation Knowledge

TF-0021 established the runtime read-model layer for MVP workspace projections. The implementation converts ordered EventEnvelope history into immutable, persona/workspace-scoped WorkspaceProjection objects for each ADR 0012 workspace route.

## Architectural Meaning

This implementation reinforces the existing semantic boundary that workspace state is derived, rebuildable, and non-authoritative. Workspace projections expose operational context, source event references, authority classifications, and derived lifecycle context without becoming lifecycle authority or canonical state.

## Replay And Projection Consequences

Workspace projections now participate in the same rebuild pattern as other projection targets. They are compatible with the ProjectionRebuildPipeline and can be regenerated from event history without live APIs, UI state, or hidden mutable storage.

## Boundary Preservation

The implementation preserves these boundaries:

- Event Ledger remains canonical truth.
- Workspace projections remain derived state.
- Persona context scopes interpretation but does not mutate facts.
- Lifecycle state is derived for context only.
- No AI or advisory summary behavior was introduced.
- No persistence or API boundary was introduced.

## Validation

Runtime validation completed successfully:

- `uv run pytest` - 128 passed
- `uv run ruff check .` - passed
- `uv run mypy src tests` - passed

## Follow-Up

Operational attention queue ordering remains separate and belongs to TF-0022. Context-aware workspace summaries remain separate and belong to TF-0023.
