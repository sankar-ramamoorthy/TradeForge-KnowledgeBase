---
title: TF-0031 React Frontend Scaffold Planning Capture
type: raw-planning-capture
status: raw
created: 2026-05-12
issue: TF-0031
milestone: M7
runtime_repo: C:\Users\bosto\dockerstuff\TradeForge
---

# TF-0031 React Frontend Scaffold Planning Capture

## Issue Discipline

- Issue ID: TF-0031
- Milestone: M7
- Title: Create React Frontend Scaffold
- Affected layer: frontend
- Linked ADRs: ADR 0021
- Impacted invariants: Workspace, UX Is Architectural

## Semantic Context

Loaded runtime semantic foundations, invariants, runtime context map, Runtime <-> KB Development Loop, UX doctrine, workspace doctrine, persona doctrine, architecture, glossary, event taxonomy, and execution contract.

Dominant task category: Workspace / Cognitive UX Work.

## Architectural Interpretation

TF-0031 establishes the frontend runtime foundation only. It must not implement workspace routing, full workspace screens, authentication/session behavior, lifecycle transitions, event writing, or projection ownership.

The React runtime is a presentation and interaction boundary over HTTP APIs. It consumes app/API contracts and must not import event store, lifecycle, persistence, replay, or projection internals.

## ADR Checkpoint

The issue register links ADR 0021, but the runtime ADR file is absent. Implementation should create ADR 0021 to define the React workspace runtime boundary before adding scaffold files.

## Design Plan

- Add ADR 0021 for React Workspace Runtime.
- Add a `frontend/` Vite React TypeScript scaffold.
- Keep initial UI minimal and explicit about derived/API-backed workspace readiness.
- Add npm scripts for local dev, build, preview, lint, and type checking.
- Update README with frontend commands.
- Update issue register when complete.

## Event, Lifecycle, Replay Impact

- Event impact: none; no canonical events are introduced.
- Lifecycle impact: none; lifecycle authority remains in service/domain/API boundaries.
- Replay impact: no replay semantics change; future UI must consume replay APIs rather than client-side truth.

## Testing Strategy

- Run existing Python tests to confirm no runtime regression.
- Run frontend install/build/type checks if dependencies are available.
- If network dependency installation is blocked, record that verification limitation explicitly.
