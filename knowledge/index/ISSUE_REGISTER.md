---
title: Issue Register
type: index
status: canonical
tags: [TradeForge, issues, architecture, roadmap]
created: 2026-05-08
updated: 2026-05-08
---

# Issue Register

## Purpose

This document is the knowledge-base issue register for semantic and architectural concerns.

It mirrors the runtime issue IDs so architectural intent can be traced to implementation work, but it is not an execution tracker.

Authority split:

- Knowledge-base issue register = semantic and architectural issue inventory
- Runtime issue register = implementation planning and local coding discipline
- GitHub issues = execution tracking

Runtime implementation issue tracking lives in:

```text
../../../../TradeForge/DOCS/ISSUE_REGISTER.md
```

---

## Register Rules

- Keep runtime issue IDs for traceability.
- Use `KB-*` issue IDs for semantic architecture work that does not map directly to a runtime implementation issue.
- Capture semantic concern before implementation detail.
- Link ADRs, canonical concepts, and workflows where available.
- Do not use this file for branch naming, scheduling, or task ownership.
- Surface contradictions with doctrine explicitly rather than silently rewriting meaning.

---

## Issue Index

| ID | Status | Milestone | Semantic Concern |
| --- | --- | --- | --- |
| KB-0001 | Planned | M1A | Canonical entity semantics |
| TF-0001 | Done | M0 | Planning discipline and architectural memory |
| TF-0002 | Planned | M1 | Event envelope and canonical event domains |
| TF-0003 | Planned | M1 | Append-only Event Ledger interface |
| TF-0004 | Planned | M1 | Early event store adapter without semantic drift |
| TF-0005 | Planned | M2 | Canonical lifecycle state model |
| TF-0006 | Planned | M2 | Deterministic lifecycle transition validation |
| TF-0007 | Planned | M2 | Lifecycle orchestration without state ownership drift |
| TF-0008 | Planned | M3 | Replay projector foundation |
| TF-0009 | Planned | M3/M4 | Persona Workspace projection read model |
| TF-0010 | Planned | M4 | Persona context model |
| TF-0011 | Planned | M5 | Market Intelligence interpretation model |
| TF-0012 | Planned | M5 | Scenario Discovery advisory model |
| TF-0013 | Planned | M6 | AI advisory boundary interfaces |
| TF-0014 | Planned | M7 | First replayable Idea-through-Review vertical slice |

---

## KB-0001: Define Canonical Entity Semantics

**Status:** Planned

**Milestone:** M1A

**Semantic Concern:** TradeForge has architectural doctrine and runtime ADR alignment, but the canonical semantic objects are not yet defined as durable entity pages.

**Architectural Concern:** Event-sourced systems depend on precise entity semantics. If entities such as trade ideas, plans, scenarios, workspaces, lifecycle events, intents, replay sessions, and review artifacts are ambiguous, event schemas and runtime domain models can drift from doctrine.

**Initial Entity Backlog:**

- [[TradeIdea]]
- [[TradeThesis]]
- [[TradePlan]]
- [[Scenario]]
- [[Workspace]]
- [[LifecycleEvent]]
- [[Persona]]
- [[ReplaySession]]
- [[ReviewArtifact]]
- [[MarketContext]]
- [[PositionIntent]]
- [[ExecutionIntent]]

**Linked ADRs:**

- [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md)
- [ADR 0002](../../../../TradeForge/DOCS/adr/0002-decision-lifecycle-engine.md)
- [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)
- [ADR 0004](../../../../TradeForge/DOCS/adr/0004-workspace-projection-model.md)
- [ADR 0005](../../../../TradeForge/DOCS/adr/0005-scenario-engine-architecture.md)
- [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)
- [ADR 0010](../../../../TradeForge/DOCS/adr/0010-market-intelligence-interpretation-layer.md)

**Linked Concepts:**

- [[GLOSSARY]]
- [[EVENT_TAXONOMY]]
- [[ARCHITECTURE]]
- [[Event Ledger]]
- [[Decision Lifecycle Engine]]
- [[Replay System]]
- [[Persona Workspace]]
- [[Market Intelligence Layer]]
- [[Scenario Discovery Engine]]

**KB Acceptance Meaning:**

- Canonical entity definition work is explicitly tracked as KB semantic architecture work.
- Entity definitions are distinguished from runtime model implementation.
- Entity pages define meaning, lifecycle role, event relationships, replay relevance, and authority boundaries.
- Runtime models may implement these concepts, but they do not become the source of semantic truth.

---

## TF-0001: Planning Discipline And Architectural Memory

**Status:** Done

**Milestone:** M0

**Semantic Concern:** TradeForge needs a durable distinction between semantic truth, runtime planning, and execution tracking.

**Architectural Concern:** Without a roadmap and issue register split, implementation tasks can become accidental architecture authority.

**Linked ADRs:**

- [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md)
- [ADR 0002](../../../../TradeForge/DOCS/adr/0002-decision-lifecycle-engine.md)
- [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)
- [ADR 0004](../../../../TradeForge/DOCS/adr/0004-workspace-projection-model.md)
- [ADR 0005](../../../../TradeForge/DOCS/adr/0005-scenario-engine-architecture.md)
- [ADR 0006](../../../../TradeForge/DOCS/adr/0006-ai-advisory-boundary-model.md)
- [ADR 0007](../../../../TradeForge/DOCS/adr/0007-anti-dashboard-ux-decision.md)
- [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)
- [ADR 0009](../../../../TradeForge/DOCS/adr/0009-persona-interpretation-model.md)
- [ADR 0010](../../../../TradeForge/DOCS/adr/0010-market-intelligence-interpretation-layer.md)

**Linked Concepts:**

- [[Semantic Truth Layer]]
- [[Architectural Memory]]
- [[Milestone Roadmap]]
- [[Issue Register]]

**KB Acceptance Meaning:**

- The knowledge base records semantic intent.
- Runtime documentation records implementation planning.
- GitHub issues remain execution tracking.

---

## TF-0002: Event Envelope And Canonical Event Domains

**Status:** Planned

**Milestone:** M1

**Semantic Concern:** Events must be modeled as immutable facts with stable domain categories.

**Architectural Concern:** The event envelope must preserve replayability, provenance, and separation between facts, interpretations, and projections.

**Linked ADRs:**

- [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md)
- [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)
- [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)

**Linked Concepts:**

- [[Event]]
- [[Event Ledger]]
- [[Canonical State]]
- [[Derived State]]
- [[Inferred State]]

**KB Acceptance Meaning:**

- Event domains align with [[EVENT_TAXONOMY]].
- Events remain facts, not interpretations.
- Event meaning is not defined by persistence infrastructure.

---

## TF-0003: Append-Only Event Store Interface

**Status:** Planned

**Milestone:** M1

**Semantic Concern:** The Event Ledger must expose append-only truth semantics.

**Architectural Concern:** Interfaces must not imply historical mutation, deletion, or projection ownership of canonical state.

**Linked ADRs:**

- [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md)
- [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)
- [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)

**Linked Concepts:**

- [[Event Ledger]]
- [[Replay System]]
- [[Canonical State]]

**KB Acceptance Meaning:**

- Event history remains immutable and replayable.
- Reads support deterministic reconstruction.
- No API shape normalizes mutable historical state.

---

## TF-0004: Early Event Store Adapter Without Semantic Drift

**Status:** Planned

**Milestone:** M1

**Semantic Concern:** Early adapters must support development without redefining Event Ledger meaning.

**Architectural Concern:** In-memory storage is acceptable as an adapter, but it must not weaken append-only semantics or become the conceptual source of truth.

**Linked ADRs:**

- [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md)
- [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)
- [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)

**Linked Concepts:**

- [[Event Ledger]]
- [[Replay System]]
- [[Derived State]]

**KB Acceptance Meaning:**

- Adapter behavior preserves append-only assumptions.
- Deterministic reads are available for early replay and tests.
- Infrastructure remains subordinate to event semantics.

---

## TF-0005: Canonical Lifecycle State Model

**Status:** Planned

**Milestone:** M2

**Semantic Concern:** The decision lifecycle must remain explicit and stageful.

**Architectural Concern:** Lifecycle state must not collapse into CRUD status fields, execution state, or UI state.

**Linked ADRs:**

- [ADR 0002](../../../../TradeForge/DOCS/adr/0002-decision-lifecycle-engine.md)
- [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)

**Linked Concepts:**

- [[Decision Lifecycle Engine]]
- [[Trade Idea]]
- [[Thesis]]
- [[Trade Plan]]
- [[Position]]
- [[Review Artifact]]

**KB Acceptance Meaning:**

- Lifecycle stages remain `Idea`, `Thesis`, `Plan`, `Approval`, `Execution`, `Position`, and `Review`.
- Stages are not merged or skipped.
- Current lifecycle state is event-derived.

---

## TF-0006: Deterministic Lifecycle Transition Validation

**Status:** Planned

**Milestone:** M2

**Semantic Concern:** Decision progression must be governed by deterministic workflow rules.

**Architectural Concern:** Invalid shortcuts must be rejected before they become ledger facts or derived workspace state.

**Linked ADRs:**

- [ADR 0002](../../../../TradeForge/DOCS/adr/0002-decision-lifecycle-engine.md)
- [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)

**Linked Concepts:**

- [[Decision Lifecycle Engine]]
- [[Decision]]
- [[Event Ledger]]
- [[Replay System]]

**KB Acceptance Meaning:**

- Valid transitions follow canonical order.
- Invalid transitions do not create canonical facts.
- Transition evaluation remains deterministic and replay-compatible.

---

## TF-0007: Lifecycle Orchestration Without State Ownership Drift

**Status:** Planned

**Milestone:** M2

**Semantic Concern:** Runtime services may coordinate lifecycle work, but they must not redefine lifecycle authority.

**Architectural Concern:** Orchestration must remain subordinate to domain rules and Event Ledger truth.

**Linked ADRs:**

- [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md)
- [ADR 0002](../../../../TradeForge/DOCS/adr/0002-decision-lifecycle-engine.md)
- [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)

**Linked Concepts:**

- [[Decision Lifecycle Engine]]
- [[Event Ledger]]
- [[Human Decision Sovereignty]]

**KB Acceptance Meaning:**

- Services orchestrate requests without becoming workflow truth.
- Accepted transitions produce event-backed facts.
- Rejected transitions leave the ledger unchanged.

---

## TF-0008: Replay Projector Foundation

**Status:** Planned

**Milestone:** M3

**Semantic Concern:** Replay must reconstruct historical system state from durable facts.

**Architectural Concern:** Projectors must consume event history deterministically and avoid live APIs, current UI state, or AI-generated summaries as replay dependencies.

**Linked ADRs:**

- [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md)
- [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)
- [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)

**Linked Concepts:**

- [[Replay System]]
- [[Replay Session]]
- [[Event Ledger]]
- [[Derived State]]

**KB Acceptance Meaning:**

- Replay output is deterministic for the same event stream.
- Projection state is derived and discardable.
- Historical reconstruction does not require external live systems.

---

## TF-0009: Persona Workspace Projection Read Model

**Status:** Planned

**Milestone:** M3/M4

**Semantic Concern:** Persona Workspaces need derived surfaces that support cognition without becoming state owners.

**Architectural Concern:** Briefing, opportunity, exposure, decision queue, and review surfaces must remain projections over canonical workflow and event state.

**Linked ADRs:**

- [ADR 0004](../../../../TradeForge/DOCS/adr/0004-workspace-projection-model.md)
- [ADR 0007](../../../../TradeForge/DOCS/adr/0007-anti-dashboard-ux-decision.md)
- [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)
- [ADR 0009](../../../../TradeForge/DOCS/adr/0009-persona-interpretation-model.md)

**Linked Concepts:**

- [[Persona Workspace]]
- [[Workspace Surface]]
- [[Briefing Surface]]
- [[Opportunity Surface]]
- [[Exposure Surface]]
- [[Decision Queue]]
- [[Review Surface]]

**KB Acceptance Meaning:**

- Workspaces remain persona-scoped operational environments.
- Surfaces are derived views, not canonical state.
- Projection rebuilds preserve replayability.

---

## TF-0010: Persona Context Model

**Status:** Planned

**Milestone:** M4

**Semantic Concern:** Personas shape interpretation and prioritization without becoming users, profiles, or state authorities.

**Architectural Concern:** Persona context must influence workflow emphasis while preserving human control and replayability.

**Linked ADRs:**

- [ADR 0004](../../../../TradeForge/DOCS/adr/0004-workspace-projection-model.md)
- [ADR 0009](../../../../TradeForge/DOCS/adr/0009-persona-interpretation-model.md)

**Linked Concepts:**

- [[Persona]]
- [[Persona Workspace]]
- [[Decision]]
- [[Replay System]]

**KB Acceptance Meaning:**

- Persona is not modeled as authentication, authorization, or UI preference.
- Persona influence remains interpretive.
- Persona-scoped operation remains compatible with historical replay.

---

## TF-0011: Market Intelligence Interpretation Model

**Status:** Planned

**Milestone:** M5

**Semantic Concern:** Market Intelligence must provide interpreted context rather than decisions.

**Architectural Concern:** Observations, interpretations, and lifecycle decisions must remain distinct.

**Linked ADRs:**

- [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)
- [ADR 0009](../../../../TradeForge/DOCS/adr/0009-persona-interpretation-model.md)
- [ADR 0010](../../../../TradeForge/DOCS/adr/0010-market-intelligence-interpretation-layer.md)

**Linked Concepts:**

- [[Market Intelligence Layer]]
- [[Inferred State]]
- [[Persona]]
- [[Decision Lifecycle Engine]]

**KB Acceptance Meaning:**

- Market Intelligence outputs remain interpreted context.
- Market observations remain distinguishable from interpretations.
- No market interpretation mutates lifecycle state.

---

## TF-0012: Scenario Discovery Advisory Model

**Status:** Planned

**Milestone:** M5

**Semantic Concern:** Scenarios are structured hypotheses, not trade decisions.

**Architectural Concern:** Scenario ranking and promotion must feed awareness and workflow intake without bypassing lifecycle controls.

**Linked ADRs:**

- [ADR 0005](../../../../TradeForge/DOCS/adr/0005-scenario-engine-architecture.md)
- [ADR 0009](../../../../TradeForge/DOCS/adr/0009-persona-interpretation-model.md)
- [ADR 0010](../../../../TradeForge/DOCS/adr/0010-market-intelligence-interpretation-layer.md)

**Linked Concepts:**

- [[Scenario]]
- [[Scenario Discovery Engine]]
- [[Opportunity Surface]]
- [[Decision Lifecycle Engine]]

**KB Acceptance Meaning:**

- Scenarios remain advisory hypotheses.
- Scenario outputs do not become trades, positions, or execution instructions.
- Promotion into workflow preserves lifecycle order.

---

## TF-0013: AI Advisory Boundary Interfaces

**Status:** Planned

**Milestone:** M6

**Semantic Concern:** AI must remain advisory and non-authoritative.

**Architectural Concern:** AI outputs must be distinguishable from canonical events, lifecycle decisions, and execution authority.

**Linked ADRs:**

- [ADR 0006](../../../../TradeForge/DOCS/adr/0006-ai-advisory-boundary-model.md)
- [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)
- [ADR 0010](../../../../TradeForge/DOCS/adr/0010-market-intelligence-interpretation-layer.md)

**Linked Concepts:**

- [[AI Advisory Boundary]]
- [[Human Decision Sovereignty]]
- [[Event Ledger]]
- [[Decision Lifecycle Engine]]
- [[Inferred State]]

**KB Acceptance Meaning:**

- AI cannot directly append canonical decision or execution events.
- AI cannot approve plans, execute trades, or transition lifecycle state.
- AI outputs preserve provenance and uncertainty where practical.

---

## TF-0014: First Replayable Idea-Through-Review Vertical Slice

**Status:** Planned

**Milestone:** M7

**Semantic Concern:** The first vertical slice must demonstrate TradeForge as a replayable decision-support system, not a CRUD tracker or autonomous trading loop.

**Architectural Concern:** The slice must connect lifecycle, event ledger, workspace projection, replay, and review without introducing broker execution or AI authority drift.

**Linked ADRs:**

- [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md)
- [ADR 0002](../../../../TradeForge/DOCS/adr/0002-decision-lifecycle-engine.md)
- [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)
- [ADR 0004](../../../../TradeForge/DOCS/adr/0004-workspace-projection-model.md)
- [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)

**Linked Concepts:**

- [[Trade Idea]]
- [[Thesis]]
- [[Trade Plan]]
- [[Decision Lifecycle Engine]]
- [[Persona Workspace]]
- [[Replay System]]
- [[Review Artifact]]

**KB Acceptance Meaning:**

- Workflow progresses through every lifecycle stage in order.
- Material transitions are event-backed.
- Workspace projection is derived from events.
- Replay reconstructs workflow and review context.
- No live broker integration or autonomous AI execution is included.
