---
title: TF-F058 Diagnosis - Root npm run dev ENOENT
type: diagnosis
status: raw
tags: [tradeforge, tf-f058, diagnosis, npm, frontend, developer-experience]
created: 2026-05-23
issues: [TF-F058]
source:
  - knowledge/raw/brainstorm-20260523-root-npm-dev-enoent.md
---

# TF-F058 Diagnosis - Root npm run dev ENOENT

## Observable Gap

Running `npm run dev` from `C:\Users\bosto\dockerstuff\TradeForge` fails with `ENOENT` because npm cannot find `C:\Users\bosto\dockerstuff\TradeForge\package.json`.

## Root Cause

The frontend package is correctly isolated under `frontend/`, but the repository root has no npm command shim. Npm therefore fails with a generic package-file error instead of starting the frontend or delegating to the frontend package.

This is an operational developer-experience bug. It is not a frontend architecture, lifecycle, replay, or domain semantic issue.

## Relevant Invariants

- Architectural Simplicity: solve with a thin root command shim rather than reorganizing packages.
- UX Is Architectural: setup and run commands should reduce operator friction.
- Domain Integrity: frontend tooling must not redefine runtime semantics or canonical state.

## Minimum Correct Scope

- Add a root `package.json` with scripts that delegate to `frontend/`.
- Include `dev`, `typecheck`, `build`, and `lint` so common commands work consistently from the root.
- Add a focused test that validates the root scripts delegate to `frontend`.
- Update README quick-start/developer setup to show root-level frontend commands.

## Out Of Scope

- Moving `frontend/package.json`.
- Duplicating React/Vite dependencies at the root.
- Changing frontend source code.
- Changing backend API, event, lifecycle, replay, advisory, or credential behavior.

## ADR Checkpoint

No new ADR is required. ADR-0011 covers development tooling conventions, and ADR-0021 keeps the React frontend runtime under `frontend/`.
