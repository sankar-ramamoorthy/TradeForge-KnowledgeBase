---
title: TF-0053 New Trade Idea Workflow — Implementation Capture
type: raw-implementation
status: raw
issue: TF-0053
milestone: M10
branch: feature/tf-0053-new-trade-idea-workflow
created: 2026-05-13
tags:
  - M10
  - lifecycle-bootstrapping
  - UX
  - demoability
  - frontend
  - implementation
---

# TF-0053: New Trade Idea Workflow — Implementation Capture

## What Was Built

### Backend

POST /lifecycle/decisions/init endpoint added to lifecycle_router in routes.py.

- NewTradeIdeaPayload: symbol (required, 1–10 chars), initial_thesis (optional, max 2000), persona_id, workspace_id
- NewTradeIdeaResponse: decision_id, symbol, event_type, timestamp
- Generates UUID4 for decision_id server-side
- Uppercases symbol automatically
- Creates entity_references: [{decision, uuid4}, {ticker, SYMBOL}]
- Payload includes symbol and initial_thesis
- Provenance: {actor: "human", source: "new-trade-idea-workflow"}
- Delegates to LifecycleOrchestrationService.transition() with LifecycleStage.IDEA
- Returns 201 on success, 409 on lifecycle rejection

### Frontend

- NewTradeIdeaModal.tsx: form with symbol input, optional thesis textarea, submit/cancel
  - Auto-uppercases symbol on input
  - Loading state during API call
  - Error display on failure
- runtime.ts: added NewTradeIdeaRequest, NewTradeIdeaResponse types + initNewTradeIdea() function
- OperatingWorkspace.tsx:
  - Added onNavigateProgrammatic prop (href: string) => void
  - Added "New Trade Idea" button (PlusCircle, prominent in surface-title row)
  - Modal open/close state
  - On success: navigates to OpportunityWorkspace with new decision_id via onNavigateProgrammatic
- App.tsx: added handleNavigateProgrammatic(), passes to OperatingWorkspace
- styles.css: modal overlay/dialog styles, form field primitives, btn-primary/btn-secondary, new-idea-trigger

## Tests

11 tests in tests/test_new_trade_idea_workflow.py — all pass.
Key discoveries:
- The lifecycle orchestration service reads all events globally (no decision_id scoping)
- Two init calls in the same event store conflict (second Idea rejected)
- This is by architecture: single-session single-decision design at this stage
- Fixed uniqueness test to use separate event stores (separate client sessions)

## Verification

- uv run pytest: 504 passed
- uv run ruff check src tests: clean
- uv run mypy src tests: clean (98 files)
- npm typecheck: clean
- npm lint: clean
- npm build: clean (246 kB JS, 18 kB CSS)

## Architectural Notes

The lifecycle orchestration service does not scope events by decision_id. This means
one session = one active decision at a time. This is a foundational constraint that
TF-0054 (persistent active decision context) and future multi-decision work will need
to address.

The onNavigateProgrammatic pattern is a clean extension of the existing onNavigate
(anchor click) pattern. Future M10 workspaces that need post-action navigation should
receive the same prop.

## Follow-up Notes

- TF-0054 should persist the active decision_id so navigation doesn't require manual
  URL parameter propagation after initial creation
- The single-decision-per-session constraint should be documented in an ADR or
  at minimum noted in ADR-0028
