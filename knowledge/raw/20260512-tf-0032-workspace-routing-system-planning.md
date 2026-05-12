---
title: TF-0032 Workspace Routing System Planning
type: raw
status: raw
created: 2026-05-12
issue: TF-0032
milestone: M7
tags:
  - TradeForge
  - runtime-kb-loop
  - frontend
  - workspace-routing
---

# TF-0032 Workspace Routing System Planning

## Runtime Issue

- Issue ID: TF-0032
- Milestone: M7 - MVP Runtime Infrastructure
- Title: Add workspace routing system
- Affected layer: frontend
- Linked ADRs: ADR 0012, ADR 0021
- Impacted invariants: Workspace, Workflow Continuity, Persona-Scoped Operation, Derived State distinction

## Semantic Context Loaded

- SEMANTIC_BOOTSTRAP.md
- INVARIANTS.md
- knowledge/index/runtime-context-map.md
- Runtime KB Development Loop
- SEMANTIC_GOVERNANCE.md
- ARCHITECTURE.md
- GLOSSARY.md
- EVENT_TAXONOMY.md
- EXECUTION_CONTRACT.md
- UX_DOCTRINE.md
- WORKSPACES.md
- PERSONAS.md
- Runtime ADR 0012
- Runtime ADR 0021

`STARTUP.md` is referenced by bootstrap/context map but was not present at `C:\Users\bosto\dockerstuff\knowledge-base\TradeForge\STARTUP.md` during planning.

## Design Plan

Implement frontend routing for the six core M8 MVP workspaces inside the React runtime boundary created by TF-0031.

The route system should:

- define typed route metadata for Operating, Opportunity, Plan Review, Active Position, Replay, and Review workspaces;
- preserve explicit persona/workflow/decision context in generated links;
- route direct browser URLs under `/workspaces/...` without treating routes as workspace truth;
- display route-specific workspace entry surfaces as derived UI projections only;
- avoid lifecycle mutation, event-store access, and client-side canonical state.

## Event Impact

No new events are introduced. Routing is presentation/navigation only and cannot append ledger facts.

## Lifecycle Impact

No lifecycle transition authority is added. Future workspace actions must continue through lifecycle/API boundaries.

## Replay Impact

Routing preserves contextual query parameters needed for replayable workspace continuity but does not define replay state.

## ADR Checkpoint

No ADR update is required. ADR 0012 establishes workspaces as operational cognition environments, and ADR 0021 reserves full workspace routing for TF-0032 under the React runtime boundary.

## Validation Strategy

- Frontend TypeScript typecheck/build.
- Existing Python routing tests where relevant.
- General runtime tests if feasible.
