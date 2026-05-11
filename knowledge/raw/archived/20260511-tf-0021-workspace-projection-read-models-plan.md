---
title: TF-0021 Workspace Projection Read Models Planning Capture
type: raw-planning-capture
status: raw
created: 2026-05-11
issue: TF-0021
milestone: M6
branch: feature/tf-0021-workspace-projection-read-models
source_runtime_repo: C:/Users/bosto/dockerstuff/TradeForge
---

# TF-0021 Workspace Projection Read Models Planning Capture

## Issue Discipline

- Issue ID: TF-0021
- Milestone: M6 - Persona Workspace Projection Layer
- Title: Implement workspace projection read models
- Affected domain layer: domain, services
- Linked ADRs: ADR 0004, ADR 0007, ADR 0008, ADR 0009, ADR 0012
- Impacted invariants: Event Sourcing, Workspace, Persona, Replay, Layer Separation
- Acceptance criteria:
  - Workspace state is derived from events and deterministic rules.
  - Workspace surfaces do not mutate canonical state.
  - Workspace context is persona-scoped.
  - Workspace projections can be rebuilt.

## Architectural Interpretation

TF-0021 translates the existing workspace state contracts into executable derived read models. Workspace projections must consume ordered Event Ledger history and produce discardable read models for the ADR 0012 workspace set. The projections are not canonical truth, do not write events, and do not authorize lifecycle transitions.

The issue belongs in the services projection/workspace layer with small domain value models where needed. It should reuse existing EventEnvelope, PersonaContext, WorkspaceRouteId, WorkspaceStateContract, and ProjectionRebuildPipeline concepts.

## Event Impact

No new event types are planned. Existing event categories are read as facts:

- persona.* for persona context references
- workspace.* for workspace context references
- decision.* for lifecycle workflow facts
- scenario.* for opportunity references
- market.* for market observation references
- execution.* for external execution facts
- review.* for review references
- system.* for doctrine/playbook references

Projection output must preserve source event references and source event types to make derived state traceable.

## Lifecycle Impact

No lifecycle transitions are introduced or changed. Lifecycle state may be derived from ordered decision/execution/review events using existing deterministic lifecycle derivation. Workspace read models may expose current lifecycle context but cannot approve, reject, execute, or complete workflows.

## Replay Impact

Workspace projections must be rebuildable from the same ordered event history. Replay compatibility depends on keeping read models immutable, deterministic, source-linked, and independent of live APIs or UI state.

## ADR Checkpoint

ADR required: no.

Reason: ADR 0004 already establishes workspace projections as derived read models; ADR 0012 already defines MVP workspace boundaries. TF-0021 implements the accepted architecture without changing projection authority, event taxonomy, lifecycle semantics, replay behavior, persistence strategy, or bounded contexts.

## Implementation Design Plan

Planned runtime changes:

- Add workspace projection read-model classes that represent derived workspace state, source event references, lifecycle context, and persona/workspace context.
- Add deterministic projector logic that filters event history by workspace contract inputs and route id.
- Add a services-layer wrapper that reads from EventStore and returns one or all workspace projections.
- Integrate projector compatibility with the existing ProjectionRebuildPipeline protocol.
- Add tests proving deterministic rebuild behavior, persona scoping, authority boundaries, lifecycle derivation, and no canonical mutation.

Expected files:

- src/services/workspace_engine/projections.py
- src/services/workspace_engine/__init__.py
- tests/test_workspace_projection_read_models.py
- DOCS/ISSUE_REGISTER.md after implementation sync
- DOCS/Milestone_Roadmap_v2.md if milestone status needs synchronization

## Boundaries

In scope:

- Derived read models for the registered workspace route set.
- Persona/workspace scoped projection context.
- Deterministic event-history projection.
- Rebuild compatibility.

Out of scope:

- React UI.
- API endpoints.
- Postgres persistence.
- Stored projections as canonical state.
- AI summaries.
- New event taxonomy.
- Attention queue implementation details reserved for TF-0022.

## Validation Plan

- uv run pytest tests/test_workspace_projection_read_models.py
- uv run pytest
- uv run ruff check .
- uv run mypy src tests

## Open Questions

None blocking. The implementation should remain intentionally lean and avoid pre-building TF-0022 attention queues or TF-0023 summaries.
