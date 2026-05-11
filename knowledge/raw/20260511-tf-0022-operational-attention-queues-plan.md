---
title: TF-0022 Operational Attention Queues Planning Capture
type: raw-planning-capture
status: raw
created: 2026-05-11
issue: TF-0022
milestone: M6
branch: feature/tf-0022-operational-attention-queues
source_runtime_repo: C:/Users/bosto/dockerstuff/TradeForge
---

# TF-0022 Operational Attention Queues Planning Capture

## Issue Discipline

- Issue ID: TF-0022
- Milestone: M6 - Persona Workspace Projection Layer
- Title: Implement operational attention queues
- Affected domain layer: domain, services
- Linked ADRs: ADR 0013
- Impacted invariants: Workflow-Centric Architecture, Workspace, Human Decision Sovereignty
- Acceptance criteria:
  - Queues are derived from lifecycle, risk, review, and workspace context.
  - Queue items explain why attention is required.
  - Queue items do not authorize execution by themselves.
  - Queue ordering is deterministic and persona-aware where applicable.

## Architectural Interpretation

Operational attention is derived queue state for what requires human attention now. It is not canonical truth, not lifecycle authority, and not execution permission. TF-0022 should build on TF-0021 workspace projections and existing lifecycle derivation instead of introducing new event authority.

The queue should represent deterministic attention items with source references, reason codes, priority metadata, and authority boundaries. Persona context may shape priority ordering through existing persona fields such as decision velocity and risk framing, but personas must not mutate state.

## Event Impact

No new event types are planned. The implementation should read existing event facts from these domains:

- decision.* for pending lifecycle workflow state
- execution.* for exposure and active position attention
- review.* for review obligations
- market.* for context changes that may require attention
- scenario.* for opportunity/watchlist attention

Queue output must preserve references to source events or source projections.

## Lifecycle Impact

No lifecycle transitions are introduced or changed. Queue items may point to lifecycle-aware next responsibilities, but must not approve, execute, reject, or complete a workflow. Lifecycle state remains derived from event history and authoritative transitions remain in lifecycle services.

## Replay Impact

Attention queues must be deterministic for the same ordered event history and persona context. They must be rebuildable and replay-compatible without live APIs, UI state, AI output, or hidden mutable state.

## ADR Checkpoint

ADR required: no.

Reason: ADR 0013 already accepts operational attention as derived queue state and defines its authority boundary. TF-0022 implements that accepted model without changing event taxonomy, lifecycle semantics, source-of-truth boundaries, or AI authority.

## Implementation Design Plan

Planned runtime changes:

- Add attention queue value models and deterministic projector under `src/services/workspace_engine/attention.py`.
- Define queue item reason/category/priority enums with stable deterministic ordering.
- Build attention queues from WorkspaceProjectionSet and existing source event references.
- Add a read service that composes EventStore history with WorkspaceProjectionSetProjector and attention queue projection.
- Export queue models through `src/services/workspace_engine/__init__.py`.
- Add tests for derived authority, source references, deterministic ordering, persona-aware priority, no event appends, rebuild compatibility, and authority boundaries.

Expected files:

- src/services/workspace_engine/attention.py
- src/services/workspace_engine/__init__.py
- tests/test_operational_attention_queues.py
- DOCS/ISSUE_REGISTER.md after implementation sync
- DOCS/Milestone_Roadmap_v2.md after implementation sync

## Boundaries

In scope:

- Derived queue models.
- Deterministic queue ordering.
- Persona-aware priority adjustments based on existing persona context.
- Source event/projection references.
- Rebuild compatibility.

Out of scope:

- AI prioritization.
- React UI.
- API endpoints.
- New event taxonomy.
- Persistence.
- Broker execution.
- Lifecycle transition orchestration.

## Validation Plan

- uv run pytest tests/test_operational_attention_queues.py
- uv run pytest
- uv run ruff check .
- uv run mypy src tests

## Open Questions

None blocking. Keep risk/review logic intentionally conservative and based on currently available event types until richer risk/review events exist.
