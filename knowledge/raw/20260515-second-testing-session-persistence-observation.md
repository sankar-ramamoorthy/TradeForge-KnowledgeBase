---
title: Second Testing Session — Persistence and Multi-Decision Navigation Observation
type: raw-capture
status: raw
tags: [TradeForge, testing, persistence, postgres, multi-decision, session-continuity, operating-workspace]
created: 2026-05-15
session: Second operational test — NVDA semiconductor momentum setup
screenshots:
  - knowledge/raw/Screenshot 2026-05-15 170901.png
  - knowledge/raw/Screenshot 2026-05-15 171040.png
  - knowledge/raw/Screenshot 2026-05-15 171300.png
---

# Second Testing Session — Persistence and Multi-Decision Navigation Observation

## What Was Tested

Second operational walkthrough using NVDA (NVIDIA Corp) as the test security.

Tested the new Armed lifecycle stage (TF-F002) successfully:
- Plan authorized → Approval
- Arm Plan modal used to declare trigger conditions
- Armed state visible in Active Position Workspace
- Declared trigger conditions displayed in amber panel
- "Confirm Trigger Met — Record Execution" button functional

NVDA trigger conditions declared:
- Enter only if NVDA reclaims and holds above 228 intraday
- Prefer confirmation with improving volume and semiconductor sector strength
- Avoid entry if NVDA breaks below 224 support
- Avoid entry during broad market risk-off conditions

Market context loaded: NVDA at 225.32, up 4.59% on 2026-05-15.

TF-F002 (Armed stage) confirmed working operationally.

---

## Observation: Session Continuity Gap

**Question surfaced during testing:**
Can I go on to another stock, log out, and come back?

**Current behavior (InMemory):**

Within a single server session:
- Multiple decisions (SMH + NVDA) can exist simultaneously
- Each decision has its own decision_id in the URL
- The in-memory event store holds all decisions concurrently

On server restart / logout:
- All data is lost — no persistence
- Must recreate decisions from scratch
- Demo flow uses seeded data to work around this

**Root cause:**
`create_app()` defaults to `InMemoryEventStore()`.
`PostgresEventStore` is already built (TF-0024, TF-0025, TF-0026) but not wired as default.

---

## Gap Analysis

### Gap 1 — No durable event persistence

Without Postgres wired, TradeForge cannot:
- Persist decisions across server restarts
- Support logout and return to prior session state
- Build a decision history

**Already solved in infrastructure:** `PostgresEventStore` exists.
**Missing:** Default wiring in `create_app()` and runtime configuration.

### Gap 2 — No multi-decision navigation surface

Even with Postgres, there is no UI to:
- List all open decisions (by ticker, stage, date)
- Navigate between an SMH decision at Armed stage and an NVDA decision at Idea stage
- See portfolio-level lifecycle status across multiple positions

The Operating Workspace has a decision queue concept but it's event-store-driven.
With in-memory: ephemeral. With Postgres: would surface all persistent decisions.

**This is the missing operational surface for a real usage session.**

### Gap 3 — No explicit session concept tied to persistence layer

"Logout and come back" implies a session. The current session model (ADR-0022, TF-0034)
implemented local session identity but without a durable event store the session
has nothing to return to.

---

## Potential Issues

TF-F-NEW-A: Wire PostgresEventStore as default runtime persistence
- Classification: architectural
- Scope: `create_app()` default, environment variable for DATABASE_URL, migration on startup
- Prerequisite for: all durable operational usage

TF-F-NEW-B: Implement multi-decision navigation in Operating Workspace
- Classification: enhancement
- Scope: decision list surface (ticker, stage, date, status), navigate to any active decision
- Depends on: TF-F-NEW-A (needs persistent decisions to list)

---

## Sequencing Consideration

These two gaps may need to be addressed **before** M10B (credential boundary).

Reasons:
1. Without persistence, every test session starts from scratch — limits testing quality
2. Multi-decision navigation is a core operational capability, not a polish item
3. Credential boundary (M10B) is important but doesn't block operational testing
4. The Armed stage, thesis/plan authoring, and revision workflows are hard to test
   meaningfully without being able to persist and return to prior decisions

Candidate milestone: **M10B** (reprioritized to Postgres + multi-decision)
Credential boundary moves to: **M10C**

Or alternatively: M10B stays credential, this becomes **M10B-pre** or **M10B.5**

Decision pending user review.

---

## Status

Raw observation. Not canonical.

Screenshots confirm TF-F002 working correctly.
Persistence and multi-decision navigation are the next testability blockers.
