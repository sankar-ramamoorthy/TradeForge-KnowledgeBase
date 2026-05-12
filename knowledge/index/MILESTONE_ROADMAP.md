---
title: Milestone Roadmap Index
type: index
status: canonical
tags: [TradeForge, roadmap, milestones, architecture, MVP]
created: 2026-05-08
updated: 2026-05-12
runtime_authority: ../../../../TradeForge/DOCS/Milestone_Roadmap_v2.md
---

# Milestone Roadmap Index

## Purpose

This document is the KB semantic index for the active TradeForge roadmap.

It is not a line-for-line mirror of the runtime roadmap. The runtime implementation planning authority is:

```text
../../../../TradeForge/DOCS/Milestone_Roadmap_v2.md
../../../../TradeForge/DOCS/ISSUE_REGISTER.md
```

This KB index records semantic intent, doctrine alignment, and current milestone meaning so future agents can understand why the roadmap is structured this way.

## Active Roadmap

Active runtime roadmap:

```text
TradeForge/DOCS/Milestone_Roadmap_v2.md
```

The old runtime roadmap file was renamed to:

```text
TradeForge/DOCS/MILESTONE_ROADMAP_DEPRECATED.md
```

It is retained only as historical planning context.

Roadmap v2 supersedes the old backend-first roadmap direction.

Core shift:

```text
backend-semantic sequencing
-> operational cognition architecture sequencing
```

## Roadmap v2 Principle

TradeForge MVP v1 should prove:

```text
replayable discretionary trading cognition
```

It should not prove autonomous trading, broker replacement, charting platform sophistication, AI orchestration, simulation, or RL.

## Milestone Summary

| Milestone | Status | Semantic Meaning |
|---|---|---|
| M0 | Done | Planning discipline and architectural memory |
| M1 | Done | Runtime scaffold and development environment |
| M1A | Done | Canonical entity definitions |
| M2 | Done | Event Ledger and canonical event model |
| M3 | Done | Decision Lifecycle Engine |
| M4 | Done | Workspace and cognitive architecture |
| M5 | Done | Replay and projection foundation |
| M6 | Done | Persona Workspace projection layer |
| M7 | Planned | MVP runtime infrastructure |
| M8 | Planned | First operational MVP vertical slice |
| M9+ | Deferred | Market/scenario/AI/adaptive layers after MVP path checkpoint |

## M4 Closeout

M4 is complete:

- TF-0014: Create workspace routing model (**Done**)
- TF-0015: Define workspace state contracts (**Done**)

TF-0014 established bounded runtime entrypoints for the ADR 0012 workspace set without making routes into workspace truth.

TF-0015 defined workspace state contracts while preserving the distinction between:

- canonical event-backed state
- derived workspace state
- inferred/advisory context
- route entry context

M5 is now complete:

- TF-0016: Implement replay projector foundation (**Done**)
- TF-0017: Implement projection rebuild pipeline (**Done**)
- TF-0018: Implement replay timeline engine (**Done**)
- TF-0019: Implement historical reconstruction pipeline (**Done**)

TF-0016 established deterministic replay projection over ordered event history. It did not introduce projection persistence, replay timelines, historical reconstruction, replay APIs, AI narration, or frontend replay workspace behavior.

TF-0017 established deterministic projection rebuild orchestration. It did not introduce projection persistence, rebuild observability events, replay timelines, historical reconstruction, or API exposure.

TF-0018 established deterministic replay timeline reconstruction over ordered event history. It did not introduce historical reconstruction composition, API exposure, AI narration, persisted timeline authority, or frontend replay workspace behavior.

TF-0019 established deterministic historical reconstruction composition over replay facts, projection state, timeline state, and an explicitly bounded inferred-state layer. It did not introduce replay APIs, frontend replay workspace behavior, persistent reconstruction authority, live API enrichment, or AI narration.

M6 is complete:

- TF-0020: Define persona context model (**Done**)
- TF-0021: Implement workspace projection read models (**Done**)
- TF-0022: Implement operational attention queues (**Done**)
- TF-0023: Implement context-aware workspace summaries (**Done**)

TF-0020 established versioned persona context as bounded interpretive input for later workspace projection and attention work. It did not introduce user identity semantics, workspace projections, attention queues, summaries, persistence, or AI persona generation.

TF-0021 established immutable persona/workspace-scoped workspace projection read models over ordered Event Ledger history.

TF-0022 established deterministic [[OperationalAttentionQueue|Operational Attention Queue]] output as derived responsibility state. Queue items explain why human attention is required without authorizing execution or lifecycle transitions.

TF-0023 established deterministic [[WorkspaceSummary|Workspace Summary]] output over workspace projections and attention queues. Summaries remain source-linked, persona-shaped, replay-compatible, and non-AI.

M6 completed the [[Persona Workspace Projection Layer]] and did not introduce projection persistence, API endpoints, React UI, broker execution, AI prioritization, or new event taxonomy.

M7 has now started:

- TF-0024: Add Postgres persistence layer (**Done**)
- TF-0025: Add Alembic migration infrastructure (**Done**)
- TF-0026: Persist canonical event ledger (**Done**)
- TF-0027: Add FastAPI application runtime (**Done**)
- TF-0028: Add lifecycle API endpoints (**Done**)
- TF-0029: Add replay API endpoints (**Done**)
- TF-0030: Add workspace projection APIs (**Done**)
- TF-0031: Create React frontend scaffold (**Done**)
- TF-0032: Add workspace routing system (**Done**)
- TF-0033: Add shared operational layout system (**Done**)

TF-0024 established local Postgres availability and infrastructure-scoped connection settings while preserving [[Event Ledger]] authority, [[Event Store Port]] boundaries, replay determinism, and projection discardability.

TF-0024 did not implement Alembic migrations, a Postgres event ledger adapter, projection persistence, API endpoints, React UI, or new runtime semantics.

TF-0025 established deterministic migration tooling and runtime ADR 0019 while preserving [[Event Ledger]] authority, replay determinism, and projection non-authority.

TF-0025 did not implement event-ledger tables, a Postgres event ledger adapter, projection tables, API endpoints, React UI, or new runtime semantics.

TF-0026 established durable canonical event persistence through a Postgres adapter behind [[Event Store Port]] and reinforced append-only Event Ledger history with database-level mutation guards.

TF-0026 did not implement event streaming infrastructure, projection persistence, API endpoints, React UI, or new runtime semantics.

TF-0027 established FastAPI as the shared HTTP runtime boundary while keeping lifecycle, replay, workspace, and persistence authority outside route handlers.

TF-0027 did not implement lifecycle APIs, replay APIs, workspace projection APIs, frontend runtime behavior, or direct persistence ownership in HTTP code.

TF-0028 established `POST /lifecycle/transitions` as an app-layer adapter over lifecycle orchestration. Transition validation and event appends remain service-layer responsibilities, and invalid transitions return explicit conflict responses rather than mutating workflow state implicitly.

TF-0028 did not introduce replay APIs, workspace projection APIs, direct database access from route handlers, or route-owned lifecycle semantics.

TF-0029 established replay HTTP reads for [[HistoricalReconstruction]] and [[ReplayTimeline]] while keeping replay derivation in deterministic replay services over shared Event Ledger history.

TF-0029 did not introduce replay workspace UI behavior, live API enrichment, AI replay narration, direct database access from route handlers, or replay authority in HTTP code.

TF-0030 established read-only workspace projection APIs over persona/workspace-scoped derived read models.

TF-0031 established the React workspace runtime boundary without full routing, session semantics, or full workspace implementation.

TF-0032 established typed React workspace routes for the six core MVP workspaces while preserving explicit persona/workflow/decision context and keeping routes as presentation entrypoints only.

TF-0033 established shared React operational layout primitives and `frontend/DESIGN.md` as a runtime translation artifact for frontend tokens and layout rationale.

## Fast MVP Path

```text
M4: lean workspace architecture
M5: replay/projection foundation
M6: persona workspace projections
M7: Postgres + API + React runtime infrastructure
M8: usable operational MVP workflow
```

## M4 Boundary Result

M4 did not include React implementation, Postgres, full workspace UI, Market Intelligence automation, Scenario Discovery, AI advisory, behavioral scoring, simulation, or RL.

Those remain outside the completed M4 boundary unless a later runtime issue or ADR explicitly pulls them forward.

## Runtime ADR Alignment

- [ADR 0012: Workspace Architecture Model](../../../../TradeForge/DOCS/adr/0012-workspace-architecture-model.md)
- [ADR 0013: Operational Attention Model](../../../../TradeForge/DOCS/adr/0013-operational-attention-model.md)
- [ADR 0014: Replay-Centric UX Model](../../../../TradeForge/DOCS/adr/0014-replay-centric-ux-model.md)
- [ADR 0018: Postgres Event Store Persistence](../../../../TradeForge/DOCS/adr/0018-postgres-event-store-persistence.md)
- [ADR 0019: Projection Persistence Architecture](../../../../TradeForge/DOCS/adr/0019-projection-persistence-architecture.md)
- [ADR 0020: FastAPI Runtime Boundary](../../../../TradeForge/DOCS/adr/0020-fastapi-runtime-boundary.md)
- [ADR 0021: React Workspace Runtime](../../../../TradeForge/DOCS/adr/0021-react-workspace-runtime.md)
- [ADR 0023: MVP Vertical Slice Definition](../../../../TradeForge/DOCS/adr/0023-mvp-vertical-slice-definition.md)

## KB Alignment

Relevant KB artifacts:

- [[Design Directory Introduction Decision]]
- [[MVP v1 Workspace Architecture Synthesis]]
- [[MVP v1 Workspace Candidate Synthesis]]
- [[MVP v1 Mockup Design Observations]]
- [[Roadmap v2 Alignment And Processing Notes]]
- [[TF-0014 Workspace Routing Model Planning Synthesis]]
- [[Implemented TF-0014 Workspace Routing Model]]
- [[TF-0016 Replay Projector Foundation]]
- [[Implemented TF-0016 Replay Projector Foundation]]
- [[TF-0017 Projection Rebuild Pipeline]]
- [[Implemented TF-0017 Projection Rebuild Pipeline]]
- [[TF-0018 Replay Timeline Engine]]
- [[Implemented TF-0018 Replay Timeline Engine]]
- [[TF-0019 Historical Reconstruction Pipeline]]
- [[Implemented TF-0019 Historical Reconstruction Pipeline]]
- [[TF-0020 Persona Context Model]]
- [[Implemented TF-0020 Persona Context Model]]
- [[Plan - TF-0021 Workspace Projection Read Models]]
- [[Implemented - TF-0021 Workspace Projection Read Models]]
- [[Plan - TF-0022 Operational Attention Queues]]
- [[Implemented - TF-0022 Operational Attention Queues]]
- [[Plan - TF-0023 Context-Aware Workspace Summaries]]
- [[Implemented - TF-0023 Context-Aware Workspace Summaries]]
- [[Plan - TF-0024 Postgres Persistence Layer]]
- [[Implemented - TF-0024 Postgres Persistence Layer]]
- [[Plan - TF-0025 Alembic Migration Infrastructure]]
- [[Implemented - TF-0025 Alembic Migration Infrastructure]]
- [[Plan - TF-0026 Postgres Event Ledger]]
- [[Implemented - TF-0026 Postgres Event Ledger]]
- [[Implemented - TF-0027 FastAPI Runtime Boundary]]
- [[Plan - TF-0028 Lifecycle API Endpoints]]
- [[Implemented - TF-0028 Lifecycle API Endpoints]]
- [[Plan - TF-0029 Replay API Endpoints]]
- [[Implemented - TF-0029 Replay API Endpoints]]
- [[Implemented - TF-0032 Workspace Routing System]]
- [[Implemented - TF-0033 Shared Operational Layout System]]
- [[Persona Workspace Projection Layer]]

Root design layer:

```text
design/
```

Status:

```text
draft design architecture, not canonical DESIGN.md
```

## Synchronization Rule

If runtime roadmap and this KB index diverge, treat the runtime roadmap as operational planning authority and update this KB index as a semantic summary.




