---
title: TF-0033 Shared Operational Layout System Planning
type: raw
status: raw
created: 2026-05-12
issue: TF-0033
milestone: M7
tags:
  - TradeForge
  - runtime-kb-loop
  - frontend
  - operational-layout
  - design-md
---

# TF-0033 Shared Operational Layout System Planning

## Runtime Issue

- Issue ID: TF-0033
- Milestone: M7 - MVP Runtime Infrastructure
- Title: Add shared operational layout system
- Affected layer: frontend
- Linked ADRs: ADR 0007, ADR 0012, ADR 0021
- Impacted invariants: UX Is Architectural, Workspace, Workflow Continuity, Derived State distinction

## Scope Update

The user requested folding the Google `design.md` concept into TF-0033. The accepted runtime placement is:

```text
frontend/DESIGN.md
```

This file is a frontend runtime design translation artifact, not semantic doctrine. KB UX doctrine, workspace doctrine, persona doctrine, and runtime ADRs remain authoritative.

## Semantic Context

Relevant active context:

- Anti-dashboard UX: layout must support cognition, not dashboard sprawl.
- Workspace architecture: workspaces are operational cognition environments, not routes or pages.
- React runtime boundary: frontend may present and interact through API contracts but must not own canonical state.

## Design Plan

Implement a bounded shared layout layer:

- Add `frontend/DESIGN.md` with frontend tokens, layout rationale, and authority boundaries.
- Add `frontend/src/operationalLayout.tsx` for shared React primitives:
  - app shell
  - workspace briefing/header
  - workspace navigation
  - context panel
  - workspace surface
  - operational surface cards
  - runtime boundary panel
- Refactor `frontend/src/App.tsx` to compose these primitives.
- Update CSS to use token-like custom properties aligned to `frontend/DESIGN.md`.

## Event Impact

No events are introduced. Layout primitives are presentation-only.

## Lifecycle Impact

No lifecycle transitions or approvals are introduced. Future action controls must remain API/lifecycle-mediated.

## Replay Impact

Layout should preserve route/context visibility and avoid hiding context needed for replayable workflow continuity.

## ADR Checkpoint

No new ADR is required. ADR 0007, ADR 0012, and ADR 0021 already govern this issue. `frontend/DESIGN.md` must reference those boundaries and avoid creating new semantic authority.

## Validation Strategy

- Frontend typecheck, lint, and production build.
- Existing workspace routing tests.
- Full Python runtime test suite if feasible.
