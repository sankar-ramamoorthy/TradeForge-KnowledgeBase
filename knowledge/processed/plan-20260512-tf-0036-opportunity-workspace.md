---
title: TF-0036 Opportunity Workspace Planning
type: processed
status: processed
created: 2026-05-12
updated: 2026-05-13
source:
  - knowledge/raw/20260512-tf-0036-opportunity-workspace-planning.md
issue: TF-0036
milestone: M8
tags:
  - TradeForge
  - runtime-kb-loop
  - frontend
  - opportunity-workspace
  - lifecycle-action
  - workspace-cognition
  - scenario-development
---

# TF-0036 Opportunity Workspace Planning

## Runtime Issue

- Issue ID: TF-0036
- Milestone: M8 — First Operational MVP Vertical Slice
- Title: Implement Opportunity Workspace
- Affected layers: frontend (primary), no new backend endpoints
- Linked ADRs: ADR 0002, ADR 0007, ADR 0012, ADR 0021, ADR 0023
- Impacted invariants: Workspace, Decision Lifecycle, Human Decision Sovereignty, Derived State Distinction, UX Is Architectural

## Semantic Context

The Opportunity Workspace is the pre-decision cognition surface. It answers:
"What candidate decisions are developing?"

This is where raw market observations become structured trade hypotheses. It is:
- pre-plan, pre-execution, pre-commitment
- the "thinking workspace" — normalizing unexecuted thinking as valuable
- governed by patience and structured reasoning, not urgency and signal generation

Key constraints from doctrine:
- Scenario-derived opportunity state is NOT a trade signal (ADR 0012, INVARIANTS).
- Promotion to thesis or plan MUST use lifecycle authority (ADR 0002).
- Context-before-action: opportunity context surfaces before lifecycle actions (ADR 0007).
- Charts support reasoning but do not dominate (brainstorm session 6 doctrine).
- Dashboard-first composition is rejected.

## What Already Exists

Backend:
- `GET /workspaces/opportunity` — returns `WorkspaceProjectionResponse` with fields:
  - `scenario_references` (CANONICAL) — from `scenario.*`, `decision.*` events
  - `opportunity_candidates` (DERIVED) — from `scenario.*`, `decision.*`, `market.*`
  - `setup_quality` (INFERRED)
  - `advisory_notes` (ADVISORY)
- `POST /lifecycle/transitions` — handles Idea → Thesis transition
- Lifecycle actions in contract: `develop-thesis` (boundary: "May request transition; does not create it.")

Frontend:
- Workspace projection fetch function (`fetchWorkspaceProjection`) already in `api/runtime.ts`
- Workspace layout primitives, routing, context management already in place

## Design Plan

**Backend:** No new endpoints required. The Opportunity Workspace consumes:
- `GET /workspaces/opportunity` (existing projection API)
- `POST /lifecycle/transitions` (existing lifecycle API)

**Frontend additions:**

1. `api/runtime.ts`: Add `LifecycleTransitionRequest` type and `postLifecycleTransition` fetch function.
   - Request includes: `requested_stage`, `timestamp`, `persona_id`, `workspace_id`, `entity_references`, `payload`, `provenance`
   - Response: `LifecycleTransitionResponse` (appended, event_type, timestamp, validation)

2. `frontend/src/workspaces/OpportunityWorkspace.tsx`:
   - Fetches `GET /workspaces/opportunity` projection on mount and context change.
   - Displays in order (context-before-action):
     a. Header: workspace title, lifecycle stage context (labeled CANONICAL)
     b. Field surfaces (labeled by authority): scenario_references, opportunity_candidates, setup_quality, advisory_notes
     c. Lifecycle action: "Develop Thesis" button (only visible when stage is Idea) — calls `POST /lifecycle/transitions` and re-fetches on success
     d. Chart context note (deferred, no charting infrastructure)
     e. Authority boundaries from projection
   - Transition state: idle / transitioning / error — action button disabled while in flight
   - On 409 conflict: shows transition error inline

3. `frontend/src/styles.css`: Add field authority surfaces (canonical/derived/inferred/advisory color coding), lifecycle action button, transition states.

4. `frontend/src/App.tsx`: Render `OpportunityWorkspace` for "opportunity" route.

## ADR Checkpoint

No new ADR required. The pattern of routing a lifecycle action through `POST /lifecycle/transitions` is the established boundary already governed by ADR 0002 and ADR 0021. No new architectural decisions or tradeoffs are introduced.

## Event Impact

A `decision.thesis_created` event may be appended to the event ledger when the operator invokes "Develop Thesis." This is the first M8 workspace to surface a user-initiated lifecycle transition. The transition goes through the lifecycle orchestration service and domain validator — no domain logic is in the frontend.

## Lifecycle Impact

Introduces UI surface for Idea → Thesis lifecycle transition. The transition is guarded by:
- The lifecycle validator (domain layer) — only valid if current stage is Idea
- The lifecycle orchestration service (services layer)
- The `POST /lifecycle/transitions` API boundary
- The workspace only initiates the request; the domain owns the transition authority.

## Replay Impact

If a thesis is created, the `decision.thesis_created` event is appended to the ledger and is replay-compatible. The workspace projection will reflect the updated lifecycle stage on re-fetch.

## Validation Strategy

- `uv run pytest` — full suite (no new backend changes)
- `uv run ruff check .`
- `uv run mypy src tests`
- `npm.cmd run typecheck`
- `npm.cmd run lint`
- `npm.cmd run build`
