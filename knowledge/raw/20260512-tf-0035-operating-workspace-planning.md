---
title: TF-0035 Operating Workspace Planning
type: raw
status: raw
created: 2026-05-12
issue: TF-0035
milestone: M8
tags:
  - TradeForge
  - runtime-kb-loop
  - frontend
  - app
  - operating-workspace
  - attention-queue
  - workspace-cognition
---

# TF-0035 Operating Workspace Planning

## Runtime Issue

- Issue ID: TF-0035
- Milestone: M8 — First Operational MVP Vertical Slice
- Title: Implement Operating Workspace
- Affected layers: frontend (primary), app (new endpoint), services (existing)
- Linked ADRs: ADR 0007, ADR 0012, ADR 0013, ADR 0021, ADR 0023
- Impacted invariants: Workspace, Workflow-Centric Architecture, UX Is Architectural, Human Decision Sovereignty, Derived State Distinction

## Semantic Context

The Operating Workspace answers the operational question: **What requires operational attention now?**

Key constraints from doctrine:
- Workspace is a derived projection surface — it does not own canonical lifecycle state.
- Context before action (ADR 0007): lifecycle stage context appears before attention items.
- Attention queue is derived state — it does not authorize execution or lifecycle transitions (ADR 0013).
- Actions route attention to the appropriate workspace via navigation links only; no lifecycle mutations occur here.
- Dashboard-first composition is explicitly rejected (ADR 0007).

The `OperationalAttentionQueueReadService` (services layer) already produces prioritized attention items from workspace projections. It is not yet exposed via API.

## Design Plan

**Backend (app layer):**
- Add `GET /workspaces/operating/attention` endpoint to `routes.py`.
- Response models: `AttentionItemResponse`, `OperationalAttentionQueueResponse`.
- Requires `PersonaContext` for the attention projector. Since a full persona registry is post-MVP, build a default MVP persona profile (SWING/BALANCED/BALANCED/MULTI_FACTOR) from query string params.
- Add `OperationalAttentionQueueReadService` to app state in `application.py`.
- Endpoint placed BEFORE `GET /workspaces/{route_id}` in route registration to avoid path conflict.

**Frontend:**
- Extend `api/runtime.ts` with `WorkspaceApiParams`, `WorkspaceProjection`, `AttentionItem`, `OperatingAttentionQueue` types and fetch functions.
- Create `frontend/src/workspaces/OperatingWorkspace.tsx`:
  - Fetches projection + attention queue in parallel (abort-safe).
  - Shows lifecycle stage context (labeled canonical).
  - Shows attention queue items ordered by priority (CRITICAL → HIGH → MEDIUM → LOW).
  - Each item: category badge, priority badge, explanation, route link to appropriate workspace.
  - Shows authority boundary notes (labeled derived).
  - Shows projection metadata footer.
- Add attention queue CSS to `styles.css` — priority-coded left borders, category/priority badges, route links.
- Update `App.tsx` to render `OperatingWorkspace` for the "operating" route; all others remain `WorkspaceSurface` placeholders.

## ADR Checkpoint

No new ADR required. ADR 0007, ADR 0012, ADR 0013, ADR 0021, and ADR 0023 already govern all decisions introduced here. The default persona profile for the attention endpoint is explicitly an MVP shortcut — not an architectural decision that requires an ADR at this stage.

## Event Impact

No events are introduced. The Operating Workspace is read-only. All state derives from existing events in the ledger through existing workspace projection and attention queue services.

## Lifecycle Impact

No lifecycle authority is added. Attention items route navigation to the appropriate workspace (plan-review, opportunity, active-position) without triggering lifecycle transitions. Lifecycle mutations remain the responsibility of the target workspaces.

## Replay Impact

The workspace projection API (`GET /workspaces/operating`) already includes source event references. The attention queue response preserves source event types per item. This preserves replay traceability without new architectural machinery.

## Validation Strategy

- `uv run pytest tests/test_operating_workspace.py` — 5 new tests covering empty queue, idea-stage item, authority boundaries, missing params, no ledger mutation
- `uv run pytest` — full suite
- `uv run ruff check .`
- `uv run mypy src tests`
- `npm.cmd run typecheck`
- `npm.cmd run lint`
- `npm.cmd run build`
