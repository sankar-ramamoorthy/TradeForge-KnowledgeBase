---
title: M10B Synthesis — Postgres Persistence and Multi-Decision Navigation
type: processed-synthesis
status: processed
tags: [TradeForge, M10B, TF-F008, TF-F009, postgres, persistence, multi-decision, operating-workspace]
created: 2026-05-15
updated: 2026-05-15
source_history:
  - knowledge/raw/20260515-second-testing-session-persistence-observation.md
  - knowledge/raw/20260515-m10b-credential-boundary-planning.md
related:
  - "[[Event Ledger Canonical Truth]]"
  - "[[Events Are Immutable]]"
  - "[[Replayability Is Foundational]]"
  - "[[Workflow-Centric Architecture]]"
issues:
  - TF-F008
  - TF-F009
---

# M10B Synthesis — Postgres Persistence and Multi-Decision Navigation

## What the Testing Revealed

Second operational session (2026-05-15, NVDA setup) confirmed TF-F002 Armed stage
working correctly. It also surfaced the persistence gap: all decision data is lost
on server restart. The operator cannot work on multiple securities, log out, and
return to prior decisions without data loss.

## What Changed

### TF-F008 — Postgres Default Persistence

`create_app()` in `src/app/api/application.py` now checks for
`TRADEFORGE_DATABASE_URL` or `TRADEFORGE_POSTGRES_HOST` at startup.

- If set: uses `PostgresEventStore()` — durable, survives restarts
- If not set: falls back to `InMemoryEventStore()` — demo and test behavior preserved

`PostgresEventStore` was already fully implemented (TF-0024 through TF-0026).
The change is a single `_default_event_store()` function. All existing tests
pass without modification (they inject `InMemoryEventStore` directly).

### TF-F009 — All-Decisions Projection + Decision List

**Backend:** `GET /lifecycle/decisions` scans all events, groups by decision_id,
derives current lifecycle stage and timestamps per decision, returns sorted list
(most recently active first).

**Frontend:** `DecisionListPanel` component in the Operating Workspace shows all
active decisions with:
- Symbol (bold)
- Lifecycle stage badge colored by stage group:
  - early (Idea, Thesis): gray
  - active (Plan, Approval): amber
  - armed (Armed): orange
  - position (Execution, Position): green
  - done (Review): muted
- "Continue →" button navigating to the correct workspace via `STAGE_TO_WORKSPACE`
- Refreshes after new trade idea is created

## Invariants Preserved

- [[Event Ledger Canonical Truth]] — Postgres event store is append-only, immutable
- [[Replayability Is Foundational]] — all events remain durable and replayable
- [[Workflow-Centric Architecture]] — decision list is derived, not stored separately

## Feedback Loop Closure

Original observation (second testing session): operator cannot go to another stock,
log out, and come back. With TRADEFORGE_DATABASE_URL set, all decisions persist across
restarts. With the decision list panel, all active decisions are visible and navigable
from the Operating Workspace.

## Verification

661 tests passing. Typecheck clean. Build clean.

With Postgres:
- Start server with TRADEFORGE_DATABASE_URL set
- Create SMH decision, advance to Armed stage
- Create NVDA decision, advance to Thesis
- Restart server
- Both decisions appear in Operating Workspace decision list
- Clicking either navigates to the correct workspace for that stage
