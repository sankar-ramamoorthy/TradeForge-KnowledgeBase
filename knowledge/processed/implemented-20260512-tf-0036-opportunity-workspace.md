---
title: TF-0036 Opportunity Workspace Implementation
type: processed
status: processed
created: 2026-05-12
updated: 2026-05-13
source:
  - knowledge/raw/20260512-tf-0036-opportunity-workspace-implementation.md
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

# TF-0036 Opportunity Workspace Implementation

## Runtime Outcome

Implemented TF-0036 on runtime branch `feature/tf-0036-opportunity-workspace`.

Runtime changes:

**Frontend API layer (`api/runtime.ts`):**
- Added `EntityReferencePayload`, `LifecycleTransitionRequest`, `LifecycleValidationResult`, `LifecycleTransitionResponse` types.
- Added `postLifecycleTransition` fetch function. This is the first M8 workspace to surface a user-initiated lifecycle transition. Error response parses the FastAPI detail body to surface the lifecycle rejection reason.

**Frontend component (`workspaces/OpportunityWorkspace.tsx`):**
- New component. Fetches `GET /workspaces/opportunity` projection on mount and context change with abort cleanup using a ref to the active controller (needed because `loadProjection` is called both from `useEffect` and after a successful transition).
- Renders in context-before-action order:
  1. Workspace header with lifecycle stage context (labeled canonical)
  2. Four field surfaces in a two-column grid, each labeled by authority (canonical/derived/inferred/advisory) and showing source event count + types
  3. "Develop Thesis" lifecycle action (only visible when stage is Idea) — calls `POST /lifecycle/transitions`, re-fetches on success, shows inline error on rejection
  4. Chart deferred note (explicit placeholder per doctrine)
  5. Authority boundaries from projection
  6. Projection metadata footer

**Frontend CSS (`styles.css`):**
- Added `.field-surfaces-grid` (2-column), `.field-surface` with authority-coded left borders (canonical=accent, derived=muted, inferred=attention, advisory=purple), `.field-authority-badge` with authority color variants, `.event-type-tag` monospace tags, `.lifecycle-action-surface`, `.lifecycle-action-btn`, `.chart-deferred-note`.

**App.tsx:**
- Added import of `OpportunityWorkspace`; renders for `activeRoute.id === "opportunity"`.

**ISSUE_REGISTER.md + Milestone_Roadmap_v2.md:**
- TF-0036 marked Done.

## First Lifecycle Action Surface

This is the first M8 workspace to surface a user-initiated lifecycle action. The pattern established:
- Action button only shown when lifecycle stage makes the transition valid (stage === "Idea" for Develop Thesis)
- Button triggers `POST /lifecycle/transitions` through the API boundary
- Domain validator at the service layer rejects invalid transitions — the 409 response message is surfaced inline
- On success, workspace projection is re-fetched to reflect the updated lifecycle state
- Button is disabled while the request is in flight

This pattern will apply to Plan Review workspace (approve/reject) and Active Position workspace in subsequent issues.

## Event Impact

A `decision.thesis_created` event may be appended to the event ledger when the operator invokes "Develop Thesis" from an Idea-stage decision context. This is the canonical lifecycle event — no bypass or shortcut. All domain validation rules apply.

## Lifecycle Impact

Introduces the first frontend-initiated lifecycle transition. The transition routes through `POST /lifecycle/transitions` → `LifecycleOrchestrationService` → domain validator. The workspace does not own lifecycle authority.

## Replay Impact

Appended `decision.thesis_created` events are replay-compatible. The workspace projection re-fetch reflects the updated lifecycle stage. Field surface data updates as new events arrive.

## Design Observation — Field Surface Pattern

The `FieldSurface` component introduces a reusable pattern for displaying workspace projection fields labeled by authority (canonical/derived/inferred/advisory). This pattern will apply to all M8 workspace implementations. Each surface shows:
- Field name (humanized)
- Authority label (color-coded)
- Authority description
- Source event count and event types

This keeps canonical/derived/inferred/advisory state visually distinguishable per the INVARIANTS.md requirement.

## Validation

Completed verification:

- `uv run pytest` — 183 passed (no new backend changes)
- `uv run ruff check .` — clean
- `uv run mypy src tests` — clean
- `npm.cmd run typecheck` — clean
- `npm.cmd run lint` — clean
- `npm.cmd run build` — clean (Vite production build)

## Follow-Up Boundary

- TF-0037 (Plan Review Workspace) should reuse the `FieldSurface` component pattern and the `postLifecycleTransition` function for Approval/Rejection actions.
- TF-0038 (Active Position Workspace) will similarly reuse these patterns.
- The chart deferred placeholder should be replaced when charting infrastructure is introduced (post-MVP scope).
- The default MVP persona profile used in the attention queue endpoint should be replaced by a proper persona registry (post-MVP scope).
