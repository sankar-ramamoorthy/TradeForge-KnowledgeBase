---
title: Issue Register Index
type: index
status: canonical
tags: [TradeForge, issues, architecture, roadmap, MVP]
created: 2026-05-08
updated: 2026-05-11
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

## Recent M4 Through Current M7 Issues

| ID | Status | Semantic Concern |
|---|---|---|
| TF-0014 | Done | Workspace routes must be bounded entrypoints, not workspace truth |
| TF-0015 | Done | Workspace state contracts must distinguish canonical, derived, inferred, advisory, and route context |
| TF-0016 | Done | Replay projection must derive deterministic, discardable state from ordered Event Ledger history |
| TF-0017 | Done | Projection rebuild pipeline must preserve projection discardability and event authority |
| TF-0018 | Done | Replay timeline must reconstruct cognition without depending on live APIs or current AI output |
| TF-0019 | Done | Historical reconstruction must preserve replay determinism and source traceability |
| TF-0020 | Done | Persona context must remain versioned, replay-safe, and bounded to interpretive influence |
| TF-0021 | Done | Workspace projection read models must remain persona/workspace-scoped, rebuildable, and non-authoritative |
| TF-0022 | Done | Operational attention queues must explain required human attention without becoming lifecycle authority |
| TF-0023 | Done | Workspace summaries must remain deterministic, source-linked, persona-shaped, and non-AI |
| TF-0024 | Done | Postgres must remain infrastructure only and preserve Event Store port, replay, and Event Ledger boundaries |

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

## TF-0017 Processing Result

TF-0017 created deterministic projection rebuild orchestration.

Semantic conclusion:

- projection rebuild is services-layer orchestration over ordered Event Ledger history
- rebuild targets consume the same ordered event tuple
- target ordering and duplicate target-name rejection preserve deterministic rebuild behavior
- rebuild reports are immutable, derived, and discardable
- rebuild orchestration does not append events, persist projection state, or create lifecycle authority
- [[Projection]] remains the stable KB concept; `ProjectionRebuildPipeline` remains implementation vocabulary

Relevant processed notes:

- [[TF-0017 Projection Rebuild Pipeline]]
- [[Implemented TF-0017 Projection Rebuild Pipeline]]

## TF-0018 Processing Result

TF-0018 created deterministic replay timeline reconstruction.

Semantic conclusion:

- [[ReplayTimeline]] is a stable derived replay concept.
- replay timeline reconstructs lifecycle, execution, review, and relevant system facts without becoming canonical truth.
- deterministic replay ordering depends on event timestamp plus source event sequence.
- timeline entries preserve source references and provenance needed for replay and review surfaces.
- timeline construction does not append events, persist authoritative state, call live APIs, use current AI output, or create lifecycle authority.

Relevant processed notes:

- [[TF-0018 Replay Timeline Engine]]
- [[Implemented TF-0018 Replay Timeline Engine]]

## TF-0019 Processing Result

TF-0019 created deterministic historical reconstruction composition.

Semantic conclusion:

- [[HistoricalReconstruction]] is a stable derived replay concept.
- historical reconstruction composes source facts, replay projection state, replay timeline state, and an explicitly bounded inferred-state layer without becoming canonical truth.
- notes and review artifacts remain source-linked to originating events.
- historical reconstruction does not append events, persist authoritative replay state, call live APIs, or create lifecycle authority.

Relevant processed notes:

- [[TF-0019 Historical Reconstruction Pipeline]]
- [[Implemented TF-0019 Historical Reconstruction Pipeline]]

## TF-0020 Processing Result

TF-0020 created the first runtime persona context foundation for M6.

Semantic conclusion:

- existing [[Persona]] semantics remain sufficient as the canonical KB concept.
- persona context must preserve explicit version identity for replay-safe interpretation.
- persona influence is bounded interpretive scope, not canonical state mutation, lifecycle authority, or execution authority.
- workspace, workflow, and decision references may be associated with persona context without making persona context a state owner.

Relevant processed notes:

- [[TF-0020 Persona Context Model]]
- [[Implemented TF-0020 Persona Context Model]]

## TF-0021 Processing Result

TF-0021 created executable workspace projection read models for the ADR 0012 workspace set.

Semantic conclusion:

- workspace projections are immutable derived read models over ordered Event Ledger history
- projections are explicitly persona/workspace scoped
- projections expose lifecycle context without owning lifecycle authority
- projection output preserves source event references and remains rebuildable
- no persistence, API, UI, AI summary behavior, or new event taxonomy was introduced

Relevant processed notes:

- [[Plan - TF-0021 Workspace Projection Read Models]]
- [[Implemented - TF-0021 Workspace Projection Read Models]]

## TF-0022 Processing Result

TF-0022 created deterministic [[OperationalAttentionQueue|Operational Attention Queue]] output as derived responsibility state.

Semantic conclusion:

- attention queues explain why human attention is required
- queue items preserve source references, lifecycle context, and workspace route targets
- persona context may shape deterministic ordering, but this influence is interpretive only
- queue output does not approve plans, authorize execution, or mutate lifecycle state
- AI prioritization remains out of scope

Relevant processed notes:

- [[Plan - TF-0022 Operational Attention Queues]]
- [[Implemented - TF-0022 Operational Attention Queues]]

## TF-0023 Processing Result

TF-0023 created deterministic [[WorkspaceSummary|Workspace Summary]] output over workspace projections and attention queues.

Semantic conclusion:

- workspace summaries are derived state and non-authoritative
- summaries preserve source inputs, source event references, and attention item references
- persona context may shape emphasis without mutating facts
- summaries are replay-compatible for the same event history and persona context
- AI-generated summaries remain out of scope

Relevant processed notes:

- [[Plan - TF-0023 Context-Aware Workspace Summaries]]
- [[Implemented - TF-0023 Context-Aware Workspace Summaries]]

## TF-0024 Processing Result

TF-0024 established the first completed M7 runtime infrastructure capability.

Semantic conclusion:

- Postgres is infrastructure, not semantic authority.
- [[Event Ledger]] remains canonical truth independent of persistence technology.
- [[Event Store Port]] remains the adapter boundary for later durable ledger work.
- replay behavior, lifecycle authority, workspace semantics, and projection authority are unchanged.
- ADR 0018 now exists in the runtime repo and captures the accepted persistence boundary.

Relevant processed notes:

- [[Plan - TF-0024 Postgres Persistence Layer]]
- [[Implemented - TF-0024 Postgres Persistence Layer]]

## M6 Processing Result

TF-0021 through TF-0023 complete the [[Persona Workspace Projection Layer]].

No contradiction was found with [[INVARIANTS]], [[ARCHITECTURE]], [[UX_DOCTRINE]], [[GLOSSARY]], or [[EVENT_TAXONOMY]].

Operational synchronization result:

- runtime issue register marks TF-0021, TF-0022, and TF-0023 Done
- runtime roadmap marks M6 Done
- KB milestone and issue indexes were updated during this processing pass
- raw source notes are archived under `knowledge/raw/archived/`

## MVP Discipline

The fast path remains:

```text
M4 -> M5 -> M6 -> M7 -> M8
```

Postgres, Alembic, FastAPI, and React belong to M7, not M4.

Market Context, Scenario Discovery, AI advisory, behavioral intelligence, simulation, and RL remain post-MVP unless a future ADR deliberately changes the roadmap.
