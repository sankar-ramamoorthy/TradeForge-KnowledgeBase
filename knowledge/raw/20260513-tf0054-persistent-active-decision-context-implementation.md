---
title: TF-0054 Persistent Active Decision Context — Implementation Capture
type: raw-implementation
status: raw
issue: TF-0054
milestone: M10
created: 2026-05-13
tags:
  - M10
  - demoability
  - active-decision
  - bug-fix
  - implementation
---

# TF-0054: Persistent Active Decision Context — Implementation Capture

## Root Cause Confirmed and Fixed

`_matches_context` in projections.py filters all events when decision_id is a
non-null non-matching string. The placeholders "decision.focus" and "workflow.current"
in LocalSessionProvider and DEFAULT_WORKSPACE_CONTEXT caused every event to be
filtered out, producing null lifecycle state and empty attention queues.

Fix: both placeholders removed. decision_id and workflow_id default to None/empty —
the filter is only applied when a real ID is present.

## What Was Changed

### Backend
- `src/app/session.py`: LocalSessionProvider — `decision_id=None, selected_workflow_id=None`
- `tests/test_session_model.py`: updated expected session values to null

### Frontend
- `frontend/src/activeDecision.ts` (new): localStorage persistence module
  - `getActiveDecision()`, `setActiveDecision()`, `clearActiveDecision()`
  - Defensive parsing — corrupt localStorage never throws
- `frontend/src/workspaceRouting.ts`: DEFAULT_WORKSPACE_CONTEXT
  - `selected_workflow_id: ""` (was "workflow.current")
  - `decision_id: ""` (was "decision.focus")
- `frontend/src/App.tsx`:
  - `useState(() => getActiveDecision())` for active decision state
  - `activeDecisionDefaults()` helper — injects decision_id, persona_id, persona_version from record
  - Revised context merge: URL > (session + activeDecision) > static defaults
  - `handleDecisionActivated()` — writes to localStorage + updates state
  - Passes `onDecisionActivated` to OperatingWorkspace
- `frontend/src/workspaces/OperatingWorkspace.tsx`:
  - New `onDecisionActivated` prop
  - Passes through to NewTradeIdeaModal
- `frontend/src/workspaces/NewTradeIdeaModal.tsx`:
  - New `personaVersion` and `onDecisionActivated` props
  - On success: builds `ActiveDecisionRecord`, calls `setActiveDecision` (localStorage) and `onDecisionActivated` (state)

## Tests

9 new tests in tests/test_active_decision_context.py — all pass.
Key tests:
- Explicitly proves M9 bug: wrong decision_id → empty queue
- Null decision_id → all events visible → queue shows items
- Real decision_id from init → queue shows items, routes to opportunity
- Session endpoint returns null (not placeholder) decision_id

## Verification
- uv run pytest: 513 passed
- uv run ruff check src tests: clean
- uv run mypy src tests: clean (99 files)
- npm typecheck: clean; npm lint: clean; npm build: clean

## Architectural Notes

The merge priority (URL > activeDecision > session > static defaults) means:
- Replay links with explicit decision_id in URL continue to work
- After creating a new idea, ALL workspaces automatically use the real UUID
- Navigating between workspaces never loses the active decision context
- Page refresh recovers from localStorage

clearActiveDecision() is implemented but not yet wired to any UI action.
A future issue (or TF-0057/0062) should clear it when review is completed.
