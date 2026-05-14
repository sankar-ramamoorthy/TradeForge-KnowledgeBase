---
title: TF-0053 New Trade Idea Workflow
type: processed
status: processed
issue: TF-0053
milestone: M10
created: 2026-05-13
tags:
  - M10
  - lifecycle-bootstrapping
  - UX
  - demoability
  - frontend
related:
  - [[TF-0054 Persistent Active Decision Context]]
  - [[TF-0055 Eliminate Manual Workspace Context Propagation]]
  - [[M10 Operational Workflow UX And Demoability]]
---

# TF-0053: New Trade Idea Workflow

## Architectural Pattern

TF-0053 closes the most critical M10 gap: starting a new decision requires no curl command.

The solution is a semantic initialization endpoint (`POST /lifecycle/decisions/init`) that:
- Generates the decision_id (UUID4) on the backend
- Constructs entity_references and calls the existing LifecycleOrchestrationService
- Returns the decision_id for frontend routing

The produced event is `decision.trade_idea_created` — identical to a manual POST to
`/lifecycle/transitions`. No new event types, no new lifecycle semantics.

## Key Decisions

**Backend ID generation (not frontend):** The decision_id is generated server-side. This keeps
ID provenance canonical and prevents frontend UUID drift.

**Semantic endpoint name:** `/lifecycle/decisions/init` rather than reusing `/lifecycle/transitions`
because the action has distinct semantic meaning — it initializes a new decision, not just advances
an existing one. This supports future M10 work (context propagation, guided flow).

**No new ADR:** Extension of ADR-0028 (Lifecycle API Endpoints). Same boundary, same pattern.

## Demoability Impact

After TF-0053:
- A user can start a new trade workflow from the UI in ~3 clicks
- The OperatingWorkspace has a visible "New Trade Idea" trigger
- Post-creation navigation routes to OpportunityWorkspace with the new decision_id

TF-0054 and TF-0055 will build on this to eliminate the manual context propagation that
currently follows the initial creation.
