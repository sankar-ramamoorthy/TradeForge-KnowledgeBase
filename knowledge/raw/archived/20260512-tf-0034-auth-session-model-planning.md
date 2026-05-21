---
title: TF-0034 Authentication And Session Model Planning
type: raw
status: raw
created: 2026-05-12
issue: TF-0034
milestone: M7
tags:
  - TradeForge
  - runtime-kb-loop
  - app
  - frontend
  - session
  - persona-boundary
---

# TF-0034 Authentication And Session Model Planning

## Runtime Issue

- Issue ID: TF-0034
- Milestone: M7 - MVP Runtime Infrastructure
- Title: Add authentication/session model
- Affected layers: app, frontend
- Linked ADRs: ADR 0022
- Impacted invariants: Persona, Workspace, Replay

## Semantic Context

Relevant active context:

- Personas are decision behavior models, not users or UI preferences.
- Workspaces are persona-scoped operational environments.
- React is a presentation boundary over FastAPI and must not own canonical state.
- Session state must support operational continuity without becoming event truth.

## Design Plan

Implement a minimal runtime session boundary:

- Add ADR 0022 to define authentication and operational identity boundaries.
- Add app-layer immutable session contracts for:
  - user identity
  - runtime session identity
  - explicit active workspace context
- Expose a read-only `GET /session` endpoint.
- Wire the frontend to read session context through the HTTP API boundary.
- Display user/session identity separately from persona/workspace context.
- Use session context as the default workspace context while preserving explicit route query overrides.

## Event Impact

No event types are introduced.

Session reads do not append events and are not canonical ledger truth.

## Lifecycle Impact

No lifecycle transitions are introduced.

Session context does not approve, reject, execute, or review decisions.

## Replay Impact

Session context supports workspace continuity but is not replay authority. Historical replay still depends on events, deterministic rules, and historical snapshots.

## ADR Checkpoint

ADR 0022 is required because TF-0034 introduces a durable boundary between user identity, runtime session identity, and persona activation.

## Validation Strategy

- Add focused tests for session contracts and the FastAPI session endpoint.
- Verify existing workspace routing behavior.
- Run frontend typecheck, lint, and build.
- Run full Python test suite.
