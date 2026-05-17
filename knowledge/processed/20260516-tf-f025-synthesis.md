---
title: TF-F025 Synthesis - Runtime API Unavailable Startup Gate
type: processed-synthesis
status: processed
tags: [TradeForge, TF-F, feedback, frontend, runtime-boundary]
created: 2026-05-16
updated: 2026-05-16
source_history:
  - knowledge/raw/20260516 feed back Bug.md
  - knowledge/raw/20260516-tf-f025-diagnosis.md
  - knowledge/raw/20260516-tf-f025-planning.md
related:
  - "[[UX Is Architectural]]"
  - "[[Architectural Simplicity]]"
issues:
  - TF-F025
---

# Feedback Identified

The frontend startup path emitted repeated proxy failures when the runtime API was unavailable, because operational components mounted before API availability had been established.

# Diagnosed Root Cause

Runtime health existed only as an informational panel concern. It was not used as an application boundary, so `/session`, workspace, attention, and lifecycle requests still fanned out while `/health` was failing.

# What Changed

- Lifted runtime health state into `App`
- Blocked workspace, sidebar, and context-rail mounting until `/health` succeeds
- Added one focused runtime-unavailable surface for the failed startup path

# Why This Scope

The minimum correct fix was frontend-only. Backend startup behavior, event sourcing, and lifecycle semantics were not implicated by the observed failure.

# Invariants Involved

- UX Is Architectural
- Architectural Simplicity

# Replay Or Lifecycle Implications

None. The change only affects frontend mounting order when the API boundary is absent.

# Source Traceability

- Raw feedback: `knowledge/raw/20260516 feed back Bug.md`
- Issue: `TF-F025`

# Closure

The original failure mode now resolves to one explicit runtime-unavailable state rather than mounting the full workspace graph while the API is down.
