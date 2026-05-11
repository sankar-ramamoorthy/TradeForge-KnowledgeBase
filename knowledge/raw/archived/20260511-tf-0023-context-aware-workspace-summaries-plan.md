---
title: TF-0023 Context-Aware Workspace Summaries Planning Capture
type: raw-planning-capture
status: raw
created: 2026-05-11
issue: TF-0023
milestone: M6
branch: feature/tf-0023-context-aware-workspace-summaries
source_runtime_repo: C:/Users/bosto/dockerstuff/TradeForge
---

# TF-0023 Context-Aware Workspace Summaries Planning Capture

## Issue Discipline

- Issue ID: TF-0023
- Milestone: M6 - Persona Workspace Projection Layer
- Title: Implement context-aware workspace summaries
- Affected domain layer: services
- Linked ADRs: ADR 0004, ADR 0009, ADR 0012
- Impacted invariants: Workspace, Persona, Derived State Distinction
- Acceptance criteria:
  - Summaries are derived and non-authoritative.
  - Summary inputs are explicit.
  - Persona context can shape emphasis without mutating facts.
  - Summaries remain replay-compatible.

## Architectural Interpretation

TF-0023 should summarize existing derived workspace context without introducing AI summaries or new authority. Summaries should consume WorkspaceProjectionSet and OperationalAttentionQueue outputs, preserve source references, and produce concise deterministic summaries suitable for workspace surfaces.

Persona context may influence emphasis, ordering, and framing labels through existing persona fields, but it must not change source facts or mutate state.

## Event Impact

No new event types are planned. Summary source inputs should remain explicit and traceable through existing workspace projection and attention queue source references.

## Lifecycle Impact

No lifecycle transitions are introduced or changed. Summaries may mention derived lifecycle context but cannot approve, reject, execute, or complete lifecycle work.

## Replay Impact

Summaries must be deterministic for the same ordered event history and persona context. They must not depend on live APIs, UI state, hidden mutable storage, or AI output.

## ADR Checkpoint

ADR required: no.

Reason: ADR 0004, ADR 0009, and ADR 0012 already define derived workspace projections, persona interpretation, and workspace architecture boundaries. TF-0023 implements a read-model summarization layer without changing source-of-truth, projection, persona, lifecycle, or event semantics.

## Implementation Design Plan

Planned runtime changes:

- Add deterministic workspace summary models and projector under `src/services/workspace_engine/summaries.py`.
- Build summaries from `WorkspaceProjectionSet` and `OperationalAttentionQueue`.
- Define summary authority, emphasis, and source-input contracts.
- Add a read service that composes EventStore history through existing projection and attention queue services.
- Export summary models through `src/services/workspace_engine/__init__.py`.
- Add tests for derived authority, explicit inputs, persona-shaped emphasis, replay determinism, source references, no event appends, and non-AI boundary.

Expected files:

- src/services/workspace_engine/summaries.py
- src/services/workspace_engine/__init__.py
- tests/test_workspace_summaries.py
- DOCS/ISSUE_REGISTER.md after implementation sync
- DOCS/Milestone_Roadmap_v2.md after implementation sync

## Boundaries

In scope:

- Deterministic workspace summaries.
- Source references and explicit input declarations.
- Persona-shaped emphasis based on existing persona context.
- Rebuild-compatible projector behavior.

Out of scope:

- AI-generated summaries.
- React UI.
- API endpoints.
- New event taxonomy.
- Persistence.
- Broker execution.
- Lifecycle orchestration.

## Validation Plan

- uv run pytest tests/test_workspace_summaries.py
- uv run pytest
- uv run ruff check .
- uv run mypy src tests

## Open Questions

None blocking. Keep summary phrasing deterministic and compact; do not attempt natural-language generation beyond stable templates.
