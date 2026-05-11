---
title: Issue Register Index
type: index
status: canonical
tags: [TradeForge, issues, architecture, roadmap, MVP]
created: 2026-05-08
updated: 2026-05-10
runtime_authority: ../../../../TradeForge/DOCS/ISSUE_REGISTER.md
---

# Issue Register Index

## Purpose

This document is the KB semantic index for the active TradeForge issue register.

It is not the execution tracker. The runtime implementation planning authority is:

```text
../../../../TradeForge/DOCS/ISSUE_REGISTER.md
```

This KB index records semantic concern, milestone meaning, and doctrine alignment. Runtime issue detail, branch names, and implementation acceptance criteria belong in the runtime repo.

## Authority Rule

If this file diverges from the runtime issue register, treat the runtime issue register as implementation authority and update this KB file as a semantic summary.

Do not mirror the full runtime issue register here.

## Active MVP Path Issue Groups

| Range | Milestone | Semantic Concern |
|---|---|---|
| TF-0001 | M0 | Planning discipline and architectural memory |
| TF-0002 to TF-0007 | M1 | Runtime scaffold without semantic drift |
| TF-0008 to TF-0010 | M2 | Event Ledger and canonical event model |
| TF-0011 to TF-0013 | M3 | Decision Lifecycle Engine |
| TF-0014 to TF-0015 | M4 | Workspace and cognitive architecture |
| TF-0016 to TF-0019 | M5 | Replay and projection foundation |
| TF-0020 to TF-0023 | M6 | Persona Workspace projection layer |
| TF-0024 to TF-0034 | M7 | MVP runtime infrastructure |
| TF-0035 to TF-0041 | M8 | First operational MVP vertical slice |
| TF-0042+ | M9+ | Deferred post-MVP market/scenario/AI/adaptive layers |

## Recent M4 And Current M5 Issues

| ID | Status | Semantic Concern |
|---|---|---|
| TF-0014 | Done | Workspace routes must be bounded entrypoints, not workspace truth |
| TF-0015 | Done | Workspace state contracts must distinguish canonical, derived, inferred, advisory, and route context |
| TF-0016 | Done | Replay projection must derive deterministic, discardable state from ordered Event Ledger history |
| TF-0017 | Planned | Projection rebuild pipeline must preserve projection discardability and event authority |
| TF-0018 | Planned | Replay timeline must reconstruct cognition without depending on live APIs or current AI output |
| TF-0019 | Planned | Historical reconstruction must preserve replay determinism and source traceability |

## TF-0014 Processing Result

TF-0014 created the runtime workspace routing model.

Semantic conclusion:

- route identifiers and paths are runtime entrypoints
- persona context is required at route construction
- selected workflow/decision context may be carried as entry context
- routes do not own workspace state, projections, lifecycle transitions, events, or UI navigation authority

Relevant processed notes:

- [[TF-0014 Workspace Routing Model Planning Synthesis]]
- [[Implemented TF-0014 Workspace Routing Model]]

## TF-0015 Processing Result

TF-0015 consumed the routing boundary without expanding it.

The stabilized semantic concern was workspace state shape:
TF-0015 created immutable workspace state contracts for the ADR 0012 workspace set.

Semantic conclusion:

- contracts define required derived read-model shape
- each state field must carry authority classification
- canonical fields are source references only, not workspace-owned truth
- allowed lifecycle-aware actions are declarations, not execution authority
- replay requirements are explicit contract obligations

Relevant processed notes:

- [[TF-0015 Workspace State Contracts Planning Synthesis]]
- [[Implemented TF-0015 Workspace State Contracts]]

## M5 Boundary

TF-0016 should consume the routing and state-contract boundaries without expanding them.

The next semantic concern is replay projector foundation:

- ordered event history must project into derived workspace state
- projections must remain discardable and rebuildable
- replay must remain deterministic
- contracts are targets, not truth

TF-0015 did not implement React, Postgres, FastAPI, replay projector infrastructure, market intelligence, AI, or behavioral scoring.

## TF-0016 Processing Result

TF-0016 created the runtime replay projector foundation.

Semantic conclusion:

- replay projection is deterministic reconstruction from ordered event history
- projection output is immutable, derived, and discardable
- replay projection may derive current lifecycle state but does not validate or advance lifecycle transitions
- service orchestration may read through the Event Store port without changing Event Ledger authority
- `ReplayProjector` remains implementation vocabulary; [[Projection]] is the stable KB concept

Relevant processed notes:

- [[TF-0016 Replay Projector Foundation]]
- [[Implemented TF-0016 Replay Projector Foundation]]

Numbering note:

- older processed notes may reference replay projector work under the previous TF-0014 assignment.
- Runtime Roadmap v2 supersedes that mapping: TF-0014 is workspace routing, TF-0015 is workspace state contracts, and TF-0016 is replay projector foundation.
TF-0016 should not implement React, Postgres, FastAPI, market intelligence, AI, or behavioral scoring.

## MVP Discipline

The fast path remains:

```text
M4 -> M5 -> M6 -> M7 -> M8
```

Postgres, Alembic, FastAPI, and React belong to M7, not M4.

Market Context, Scenario Discovery, AI advisory, behavioral intelligence, simulation, and RL remain post-MVP unless a future ADR deliberately changes the roadmap.
