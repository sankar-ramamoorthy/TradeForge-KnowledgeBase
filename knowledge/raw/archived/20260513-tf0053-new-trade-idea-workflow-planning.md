---
title: TF-0053 New Trade Idea Workflow — Planning Capture
type: raw-planning
status: raw
issue: TF-0053
milestone: M10
branch: feature/tf-0053-new-trade-idea-workflow
created: 2026-05-13
tags:
  - M10
  - UX
  - demoability
  - lifecycle-bootstrapping
  - frontend
  - planning
---

# TF-0053: Implement New Trade Idea Workflow — Planning Capture

## Operational State

- M10 is now "Operational Workflow UX And Demoability" (roadmap updated, AI advisory shifted to M11)
- TF-0053 is the first M10 issue
- Branch: feature/tf-0053-new-trade-idea-workflow

## Problem Statement

Currently, starting a new trade decision requires a raw API call (curl/manual POST to /lifecycle/transitions with a hand-constructed payload). There is no UI entry point. This is the primary operational gap blocking demoability.

## Selected Issue

TF-0053 — Implement New Trade Idea Workflow (M10, lifecycle bootstrapping group)

## Scope

**In scope:**
- POST /lifecycle/decisions/init endpoint (new, semantic lifecycle initialization)
- NewTradeIdeaPayload + NewTradeIdeaResponse Pydantic models
- NewTradeIdeaModal.tsx React component (symbol input, optional thesis notes, submit/cancel)
- API client function: initNewTradeIdea() in runtime.ts
- "New Trade Idea" button trigger in OperatingWorkspace
- Programmatic navigation prop (onNavigateProgrammatic) for post-creation routing
- Post-creation navigation to OpportunityWorkspace with new decision_id
- Tests: backend endpoint coverage + integration tests
- Modal CSS styles

**Out of scope:**
- Persistent active decision context (TF-0054)
- Eliminating manual workspace context propagation (TF-0055)
- Guided lifecycle navigation (TF-0056)
- Demo mode infrastructure (TF-0058+)
- Symbol validation against live market data

## Architecture Reasoning

The POST /lifecycle/decisions/init endpoint is a semantic wrapper around the existing
lifecycle orchestration service. It:
- Generates the decision_id UUID on the backend (not frontend) — keeps ID generation canonical
- Constructs the proper entity_references shape automatically
- Uses the existing LifecycleOrchestrationService.transition() with requested_stage=Idea
- Returns the decision_id so the frontend can route to the OpportunityWorkspace

Entity references for the new event:
- {entity_type: "decision", entity_id: <uuid4>}
- {entity_type: "ticker", entity_id: <SYMBOL.upper()>}

Payload: {symbol: <symbol>, initial_thesis: <optional>}

Provenance: {actor: "human", source: "new-trade-idea-workflow"}

This preserves all existing lifecycle integrity — the event produced is identical in structure
to a manually-crafted POST /lifecycle/transitions call, but the endpoint handles the
boilerplate so the UI doesn't need to.

## ADR Checkpoint

ADR required: no

Reason: TF-0053 extends the existing lifecycle API boundary (ADR-0028) with a new semantic
endpoint. The decision to generate IDs on the backend vs. frontend is a design choice within
the existing API boundary, not a new architectural pattern. ADR-0028 will be noted as the
governing ADR. No new architectural tradeoffs are introduced.

## Event and Replay Check

No new event types needed. The existing "decision.trade_idea_created" event is emitted.
The new endpoint produces identical event structure to a manual transition POST.
Replay is unaffected — the event is canonical, the endpoint is just a UI-friendly wrapper.

## Implementation Plan

### Backend

1. routes.py additions:
   - NewTradeIdeaPayload(BaseModel): symbol, initial_thesis (optional), persona_id, workspace_id
   - NewTradeIdeaResponse(BaseModel): decision_id, symbol, event_type, timestamp
   - POST /lifecycle/decisions/init endpoint (under lifecycle_router)
   - Uses LifecycleOrchestrationService, generates uuid4 for decision_id
   - Returns 201 on success, 409 if lifecycle rejects

### Frontend

1. frontend/src/api/runtime.ts additions:
   - NewTradeIdeaRequest type
   - NewTradeIdeaResponse type
   - initNewTradeIdea(request, signal) function

2. frontend/src/workspaces/NewTradeIdeaModal.tsx (new):
   - symbol input (required, auto-uppercase on blur)
   - initial_thesis textarea (optional)
   - Submit / Cancel buttons
   - Loading state
   - Error display

3. frontend/src/workspaces/OperatingWorkspace.tsx modifications:
   - Add onNavigateProgrammatic prop
   - Add "New Trade Idea" button (+ icon, prominent, near workspace header)
   - Modal open/close state
   - On modal success: navigate to OpportunityWorkspace with new decision_id

4. frontend/src/App.tsx modifications:
   - Add handleNavigateProgrammatic function
   - Pass to OperatingWorkspace as onNavigateProgrammatic

5. frontend/src/styles.css additions:
   - .modal-overlay, .modal-dialog, .modal-header, .modal-body, .modal-footer styles
   - .form-field, .form-label, .form-input, .form-textarea styles
   - .new-idea-trigger styles

### Tests

tests/test_new_trade_idea_workflow.py:
- test_init_creates_idea_event
- test_init_returns_non_empty_decision_id
- test_init_symbol_captured_in_entity_references
- test_init_missing_symbol_returns_422
- test_init_missing_persona_id_returns_422
- test_init_workspace_projection_reflects_idea_stage
- test_init_attention_queue_shows_idea_item_after_creation

## Validation Plan

uv run pytest tests/test_new_trade_idea_workflow.py
uv run pytest
uv run ruff check .
uv run mypy src tests
npm.cmd run typecheck
npm.cmd run lint
npm.cmd run build

## Open Questions

None — scope is well-bounded and patterns are established.

## Follow-up Notes

- TF-0054 (Persistent Active Decision Context) will eliminate the need to pass
  decision_id in every workspace URL after initial creation
- TF-0055 will remove manual query param propagation
- These two issues will make TF-0053's modal feel much smoother in practice
