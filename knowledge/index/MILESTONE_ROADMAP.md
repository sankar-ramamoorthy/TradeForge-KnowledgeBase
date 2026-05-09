---
title: Milestone Roadmap
type: index
status: canonical
tags: [TradeForge, roadmap, milestones, architecture]
created: 2026-05-08
updated: 2026-05-08
---

# Milestone Roadmap

## Purpose

This document is the knowledge-base milestone roadmap for TradeForge.

It records long-term semantic and architectural milestone intent: why each milestone matters, what conceptual capability it stabilizes, and which ADRs, issues, entities, and workflows it connects.

This roadmap is not the runtime implementation plan.

Authority split:

- Knowledge-base roadmap = why and what
- Runtime roadmap = how and when
- GitHub issues = execution tracking

Runtime implementation planning lives in:

```text
../../../../TradeForge/DOCS/MILESTONE_ROADMAP.md
../../../../TradeForge/DOCS/ISSUE_REGISTER.md
```

---

## Alignment References

Core doctrine:

- [[INVARIANTS]]
- [[ARCHITECTURE]]
- [[GLOSSARY]]
- [[EVENT_TAXONOMY]]
- [[UX_DOCTRINE]]

Runtime ADR alignment references:

- [ADR 0001: Event Sourcing Core Model](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md)
- [ADR 0002: Decision Lifecycle Engine](../../../../TradeForge/DOCS/adr/0002-decision-lifecycle-engine.md)
- [ADR 0003: Canonical Event Taxonomy](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)
- [ADR 0004: Workspace Projection Model](../../../../TradeForge/DOCS/adr/0004-workspace-projection-model.md)
- [ADR 0005: Scenario Engine Architecture](../../../../TradeForge/DOCS/adr/0005-scenario-engine-architecture.md)
- [ADR 0006: AI Advisory Boundary Model](../../../../TradeForge/DOCS/adr/0006-ai-advisory-boundary-model.md)
- [ADR 0007: Anti-Dashboard UX Decision](../../../../TradeForge/DOCS/adr/0007-anti-dashboard-ux-decision.md)
- [ADR 0008: Replay System Design](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)
- [ADR 0009: Persona Interpretation Model](../../../../TradeForge/DOCS/adr/0009-persona-interpretation-model.md)
- [ADR 0010: Market Intelligence Interpretation Layer](../../../../TradeForge/DOCS/adr/0010-market-intelligence-interpretation-layer.md)
- [ADR 0011: Runtime Development Environment](../../../../TradeForge/DOCS/adr/0011-runtime-development-environment.md)

Note: runtime ADRs are alignment references until corresponding KB ADR, ontology, entity, or workflow pages are explicitly promoted.

---

## M0: Planning Discipline And Architectural Memory

**Status:** Done

**Semantic Intent:** Establish durable planning discipline before implementation expands.

**Architectural Significance:** TradeForge needs a persistent distinction between semantic truth, runtime documentation, and execution tracking so architectural memory does not collapse into code tasks.

**Canonical Concepts:**

- [[Architectural Memory]]
- [[Issue Register]]
- [[Milestone Roadmap]]
- [[Semantic Truth Layer]]

**Linked Runtime Issues:**

- TF-0001: Establish milestone roadmap and issue register

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
- [ADR 0011](../../../../TradeForge/DOCS/adr/0011-runtime-development-environment.md)

**KB Acceptance Meaning:**

- The KB records why roadmap and issue discipline exists.
- Runtime documentation remains the implementation planning authority.
- GitHub issues remain execution tracking rather than semantic truth.

---

## M1: Runtime Scaffold And Developer Environment

**Status:** Done

**Semantic Intent:** Establish a reproducible implementation environment before runtime domain code begins.

**Architectural Significance:** Runtime tooling supports implementation consistency, but it does not define TradeForge semantics. Python, `uv`, Docker, Docker Compose, pytest, lint, type, and README setup conventions must remain infrastructure concerns subordinate to the knowledge-base truth layer.

**Canonical Concepts:**

- [[Semantic Truth Layer]]
- [[Architectural Memory]]
- [[Event Ledger]]
- [[Decision Lifecycle Engine]]

**Linked Runtime Issues:**

- TF-0002: Create Python project scaffold with pyproject.toml and uv (**Done**)
- TF-0003: Add Dockerfile using uv Python 3.12 slim base image (**Done**)
- TF-0004: Add docker-compose.yml for local development (**Done**)
- TF-0005: Add pytest baseline and test command (**Done**)
- TF-0006: Add lint, type, and dev command conventions (**Done**)
- TF-0007: Add README developer setup section (**Done**)

**Linked ADRs:**

- [ADR 0011](../../../../TradeForge/DOCS/adr/0011-runtime-development-environment.md)

**KB Acceptance Meaning:**

- Runtime environment decisions are acknowledged as implementation alignment, not semantic authority.
- Tooling does not define event semantics, lifecycle rules, persona meaning, workspace behavior, replay rules, or AI authority.
- Domain implementation can rely on repeatable local development commands.

---

## M1A: Canonical Entity Definitions

**Status:** Done

**Semantic Intent:** Define the durable semantic objects that events, workflows, projections, replay, and review refer to.

**Architectural Significance:** ADRs establish doctrine and boundaries, but entity definitions stabilize the meaning of the system's core objects. In an event-sourced system, ambiguous entity semantics create downstream ambiguity in event design, lifecycle rules, projections, and replay.

**Canonical Entities To Define:**

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

**Linked KB Issues:**

- KB-0001: Define canonical entity semantics

**Linked ADRs:**

- [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md)
- [ADR 0002](../../../../TradeForge/DOCS/adr/0002-decision-lifecycle-engine.md)
- [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)
- [ADR 0004](../../../../TradeForge/DOCS/adr/0004-workspace-projection-model.md)
- [ADR 0005](../../../../TradeForge/DOCS/adr/0005-scenario-engine-architecture.md)
- [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)
- [ADR 0010](../../../../TradeForge/DOCS/adr/0010-market-intelligence-interpretation-layer.md)

**KB Acceptance Meaning:**

- Canonical entity definitions exist before runtime models are treated as semantic authority.
- Entity definitions distinguish canonical, derived, inferred, and advisory objects.
- Events and lifecycle rules can reference stable entity meanings.
- Runtime implementation remains free to choose code structure, but not to redefine entity semantics silently.

---

## M2: Event Ledger And Canonical Event Model

**Status:** In Progress

**Semantic Intent:** Stabilize the Event Ledger as the canonical truth layer.

**Architectural Significance:** All durable state must derive from immutable, replayable events. This milestone protects the distinction between facts, derived state, inferred state, and advisory interpretation.

**Canonical Concepts:**

- [[Event Ledger]]
- [[Event]]
- [[Canonical State]]
- [[Derived State]]
- [[Replay System]]

**Linked Runtime Issues:**

- TF-0008: Define event envelope and canonical event domains (**Done**)
- TF-0009: Define append-only event store interface (**Planned**)
- TF-0010: Implement in-memory event store adapter (**Planned**)

**Linked ADRs:**

- [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md)
- [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)
- [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)

**KB Acceptance Meaning:**

- Event semantics align with [[EVENT_TAXONOMY]].
- Events remain facts, not interpretations.
- The Event Ledger is preserved as canonical truth.

---

## M3: Decision Lifecycle Engine

**Status:** Planned

**Semantic Intent:** Stabilize the workflow authority for decision progression.

**Architectural Significance:** TradeForge is workflow-centric, not CRUD-centric. The Decision Lifecycle Engine owns state transitions and prevents lifecycle collapse.

**Canonical Concepts:**

- [[Decision Lifecycle Engine]]
- [[Decision]]
- [[Trade Idea]]
- [[Thesis]]
- [[Trade Plan]]
- [[Position]]
- [[Review Artifact]]

**Linked Runtime Issues:**

- TF-0011: Define lifecycle state model
- TF-0012: Implement lifecycle transition validator
- TF-0013: Implement lifecycle orchestration service

**Linked ADRs:**

- [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md)
- [ADR 0002](../../../../TradeForge/DOCS/adr/0002-decision-lifecycle-engine.md)
- [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)

**KB Acceptance Meaning:**

- The canonical lifecycle remains `Idea -> Thesis -> Plan -> Approval -> Execution -> Position -> Review`.
- Human approval and explicit transitions remain mandatory.
- No subsystem other than the lifecycle engine owns workflow state.

---

## M4: Replay And Projection Foundation

**Status:** Planned

**Semantic Intent:** Make historical reconstruction a foundational capability.

**Architectural Significance:** Replay protects auditability, reflection, and learning. Projections are useful read models, but they are discardable and non-authoritative.

**Canonical Concepts:**

- [[Replay System]]
- [[Replay Session]]
- [[Event Ledger]]
- [[Derived State]]
- [[Persona Workspace]]

**Linked Runtime Issues:**

- TF-0014: Implement replay projector foundation
- TF-0015: Implement workspace projection read model

**Linked ADRs:**

- [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md)
- [ADR 0004](../../../../TradeForge/DOCS/adr/0004-workspace-projection-model.md)
- [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)

**KB Acceptance Meaning:**

- Replay depends on immutable events, deterministic rules, and historical snapshots.
- Replay does not depend on live APIs or current UI state.
- Projection state remains derived and rebuildable.

---

## M5: Persona Workspace Projection Model

**Status:** Planned

**Semantic Intent:** Define workspaces as persona-scoped operational environments.

**Architectural Significance:** Persona Workspaces preserve cognitive continuity and prevent dashboard-centric drift. Workspace surfaces organize decision context without owning canonical state.

**Canonical Concepts:**

- [[Persona]]
- [[Persona Workspace]]
- [[Workspace Surface]]
- [[Briefing Surface]]
- [[Opportunity Surface]]
- [[Exposure Surface]]
- [[Decision Queue]]
- [[Review Surface]]

**Linked Runtime Issues:**

- TF-0015: Implement workspace projection read model
- TF-0016: Define persona context model

**Linked ADRs:**

- [ADR 0004](../../../../TradeForge/DOCS/adr/0004-workspace-projection-model.md)
- [ADR 0007](../../../../TradeForge/DOCS/adr/0007-anti-dashboard-ux-decision.md)
- [ADR 0009](../../../../TradeForge/DOCS/adr/0009-persona-interpretation-model.md)

**KB Acceptance Meaning:**

- Workspaces are operational environments, not tabs or dashboards.
- Personas shape interpretation without mutating canonical state.
- UX remains context-first, workflow-first, and review-aware.

---

## M6: Market Intelligence And Scenario Discovery

**Status:** Planned

**Semantic Intent:** Stabilize advisory interpretation layers for market context and candidate scenarios.

**Architectural Significance:** Market Intelligence and Scenario Discovery improve situational awareness without becoming decision authority. They surface interpreted context and hypotheses, not commands.

**Canonical Concepts:**

- [[Market Intelligence Layer]]
- [[Scenario Discovery Engine]]
- [[Scenario]]
- [[Inferred State]]
- [[Decision Lifecycle Engine]]

**Linked Runtime Issues:**

- TF-0017: Define market intelligence interpretation model
- TF-0018: Define scenario discovery advisory model

**Linked ADRs:**

- [ADR 0005](../../../../TradeForge/DOCS/adr/0005-scenario-engine-architecture.md)
- [ADR 0009](../../../../TradeForge/DOCS/adr/0009-persona-interpretation-model.md)
- [ADR 0010](../../../../TradeForge/DOCS/adr/0010-market-intelligence-interpretation-layer.md)

**KB Acceptance Meaning:**

- Market Intelligence provides interpreted context, not decisions.
- Scenarios remain hypotheses, not trades, positions, or execution instructions.
- Advisory outputs cannot bypass lifecycle controls.

---

## M7: AI Advisory Boundary

**Status:** Planned

**Semantic Intent:** Preserve human decision sovereignty when AI assistance is introduced.

**Architectural Significance:** AI may summarize, rank, cluster, contextualize, and assist review, but it must not become an authority over lifecycle state, ledger truth, approval, or execution.

**Canonical Concepts:**

- [[AI Advisory Boundary]]
- [[Human Decision Sovereignty]]
- [[Decision Lifecycle Engine]]
- [[Event Ledger]]
- [[Review Artifact]]

**Linked Runtime Issues:**

- TF-0019: Define AI advisory boundary interfaces

**Linked ADRs:**

- [ADR 0006](../../../../TradeForge/DOCS/adr/0006-ai-advisory-boundary-model.md)
- [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)
- [ADR 0010](../../../../TradeForge/DOCS/adr/0010-market-intelligence-interpretation-layer.md)

**KB Acceptance Meaning:**

- AI outputs remain advisory artifacts.
- AI cannot write canonical events directly.
- AI cannot approve plans, execute trades, or transition lifecycle state.
- AI uncertainty and provenance remain visible where practical.

---

## M8: First Replayable Vertical Slice

**Status:** Planned

**Semantic Intent:** Demonstrate the first complete decision workflow as a replayable system narrative.

**Architectural Significance:** The first vertical slice should prove that TradeForge is a decision-support system built from event-backed workflow transitions, persona-scoped workspace projections, and reviewable history.

**Canonical Concepts:**

- [[Decision Lifecycle Engine]]
- [[Event Ledger]]
- [[Persona Workspace]]
- [[Replay System]]
- [[Review Artifact]]

**Linked Runtime Issues:**

- TF-0020: Implement first vertical slice from Idea through Review replay

**Linked ADRs:**

- [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md)
- [ADR 0002](../../../../TradeForge/DOCS/adr/0002-decision-lifecycle-engine.md)
- [ADR 0004](../../../../TradeForge/DOCS/adr/0004-workspace-projection-model.md)
- [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)

**KB Acceptance Meaning:**

- The vertical slice demonstrates lifecycle continuity from Idea through Review.
- Every material state change is event-backed.
- Workspace state is derived, not canonical.
- Review and replay are included as first-class capabilities.
- No live broker execution or autonomous AI control is introduced.
