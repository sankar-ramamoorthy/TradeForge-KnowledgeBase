---
title: TF-F058 Synthesis - Root npm Frontend Scripts
type: processed-synthesis
status: processed
tags: [TradeForge, TF-F058, feedback, npm, frontend, developer-experience]
created: 2026-05-23
updated: 2026-05-23
source_history:
  - knowledge/raw/archived/brainstorm-20260523-root-npm-dev-enoent.md
  - knowledge/raw/archived/20260523-tf-f058-diagnosis.md
  - knowledge/raw/archived/20260523-tf-f058-planning.md
related:
  - "[[UX Is Architectural]]"
issues:
  - TF-F058
---

# TF-F058 Synthesis - Root npm Frontend Scripts

## Feedback Identified

Running `npm run dev` from the TradeForge repository root failed with npm `ENOENT` because the root did not have a `package.json`. The actual frontend package exists under `frontend/`.

## Root Cause

Frontend tooling is intentionally isolated under `frontend/`, but the root repository lacked a thin command shim for common operator commands. The failure was a developer-experience bug rather than a frontend runtime bug.

## Change Made

- Added a root `package.json` with scripts that delegate to `frontend/`.
- Added `install:frontend`, `dev`, `typecheck`, `build`, `lint`, and `preview` delegations.
- Kept frontend dependencies exclusively in `frontend/package.json`.
- Updated README quick-start and developer setup commands to use root-level npm scripts.
- Added focused pytest coverage for root script shape.

## Invariants Involved

- Architectural simplicity: no package relocation or dependency duplication.
- UX is architectural: common startup commands now fail less abruptly.
- Domain integrity: no runtime, lifecycle, replay, advisory, or event behavior changed.

## Replay And Lifecycle Implications

None.

## Promotion Assessment

No entity, topic, workflow, playbook, prompt, or ADR promotion is required. The
change is a developer-experience command shim and does not affect TradeForge
domain semantics.

## Verification

- `uv run pytest tests\test_frontend_root_scripts.py`
- `npm.cmd run dev -- --help`
- `uv run pytest`
- `npm.cmd run typecheck`
- `npm.cmd run build`

## Closure State

Closed. The source command path now resolves from the repository root and reaches the frontend Vite command.
