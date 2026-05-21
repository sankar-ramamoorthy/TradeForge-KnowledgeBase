---
title: TF-0054 Persistent Active Decision Context — Planning Capture
type: raw-planning
status: raw
issue: TF-0054
milestone: M10
branch: feature/tf-0054-persistent-active-decision-context
created: 2026-05-13
tags:
  - M10
  - demoability
  - active-decision
  - context-persistence
  - planning
---

# TF-0054: Persistent Active Decision Context — Planning Capture

## Root Cause of M9 Demo Failure (confirmed)

`_matches_context` in `src/services/workspace_engine/projections.py` filters events
by decision_id when context.decision_id is not None:

```python
if context.decision_id is not None and not _event_references_entity(
    event, entity_type="decision", entity_id=context.decision_id
):
    return False
```

The frontend's DEFAULT_WORKSPACE_CONTEXT has `decision_id: "decision.focus"` and
`selected_workflow_id: "workflow.current"`. These are placeholder strings that DON'T
match any real event entity_references. Result: all events are filtered out, lifecycle
state is None, attention queue is empty.

Same filter logic exists for workflow_id. The `LocalSessionProvider` backend default
also returns these same placeholder values, reinforcing the problem through the session
endpoint.

If `context.decision_id is None`, the filter is skipped and ALL events are visible.
The fix: stop sending fake placeholder IDs in workspace API calls.

## Two-Part Fix

### Part 1 — Remove placeholder defaults
- Backend: `LocalSessionProvider` — `decision_id=None`, `selected_workflow_id=None`
- Frontend: `DEFAULT_WORKSPACE_CONTEXT` — `decision_id: ""`, `selected_workflow_id: ""`
  (empty strings are mapped to `undefined` in API param builders)

### Part 2 — Persist real decision_id from TF-0053 init
- `frontend/src/activeDecision.ts` — localStorage-backed persistence
- `App.tsx` — read on mount, expose `onDecisionActivated` callback, merge into context
- `OperatingWorkspace.tsx` — propagate `onDecisionActivated` to modal
- `NewTradeIdeaModal.tsx` — call it after successful creation

## Context Merge Priority (revised)

URL params > active decision (localStorage) > session defaults > static defaults

URL params always win (supports replay links, deep links).
Active decision fills in automatically when no URL override.
Session defaults provide persona/workspace context (not decision_id).
Static defaults never include fake decision_id.

## ADR Checkpoint

ADR required: no. This is a bug fix to placeholder data combined with a frontend
persistence mechanism. No new architectural boundaries or invariants.

## Event and Replay Check

No event model changes. Backend is stateless. The active decision context is a
frontend session artifact. Replay links that include explicit decision_id in the URL
continue to work (URL params take priority).

## Implementation Plan

### Backend
1. `src/app/session.py`: `LocalSessionProvider` — `selected_workflow_id=None, decision_id=None`

### Frontend
1. `frontend/src/activeDecision.ts` (new):
   - `ActiveDecisionRecord` type: `{decision_id, symbol, persona_id, persona_version, created_at}`
   - `getActiveDecision(): ActiveDecisionRecord | null`
   - `setActiveDecision(record: ActiveDecisionRecord): void`
   - `clearActiveDecision(): void`
   - Key: "tradeforge.active_decision"

2. `frontend/src/workspaceRouting.ts`:
   - `DEFAULT_WORKSPACE_CONTEXT.selected_workflow_id` → `""`
   - `DEFAULT_WORKSPACE_CONTEXT.decision_id` → `""`

3. `frontend/src/App.tsx`:
   - `useState<ActiveDecisionRecord | null>(() => getActiveDecision())`
   - `handleDecisionActivated(record)` — calls `setActiveDecision(record)` + setState
   - Context merge: `{...DEFAULT_WORKSPACE_CONTEXT, ...sessionDefaults, ...activeDecisionDefaults, ...fromUrl}`
   - Pass `onDecisionActivated` to `OperatingWorkspace`

4. `frontend/src/workspaces/OperatingWorkspace.tsx`:
   - Add `onDecisionActivated: (record: ActiveDecisionRecord) => void` prop
   - Pass to `NewTradeIdeaModal`

5. `frontend/src/workspaces/NewTradeIdeaModal.tsx`:
   - Accept `onDecisionActivated` prop
   - Call it with `{decision_id, symbol, persona_id, persona_version, created_at}` before `onCreated`

6. `frontend/src/activeDecision.ts` type exported and imported where needed

### Tests
`tests/test_active_decision_context.py`:
- `test_mismatched_decision_id_produces_empty_lifecycle` — proves the M9 bug
- `test_null_decision_id_sees_all_events` — proves the fix (skip filter when None)
- `test_session_endpoint_decision_id_is_null` — session returns null, not placeholder
- `test_init_then_attention_queue_with_real_decision_id` — end-to-end working flow

## Validation Plan

uv run pytest tests/test_active_decision_context.py
uv run pytest
uv run ruff check src tests
uv run mypy src tests
npm.cmd run typecheck
npm.cmd run lint
npm.cmd run build
