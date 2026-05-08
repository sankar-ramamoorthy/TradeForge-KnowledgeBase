# TradeForge — Architecture

## Purpose

This document defines the structural architecture of TradeForge:
how the system is composed, how responsibilities are separated, and how information flows between subsystems.

It translates system invariants into concrete architectural layers and bounded contexts.

This is the primary reference for:
- system design
- module boundaries
- data flow
- ownership of state
- interaction between subsystems

---

# High-Level Architecture

TradeForge is a **persona-driven, workflow-centric, event-sourced decision support system**.

The system is structured as a modular monolith with clear bounded contexts.

Core architecture:

```text
Personas
    ↓
Persona Workspaces
    ↓
Market Intelligence Layer
    ↓
Scenario Discovery Engine
    ↓
Decision Lifecycle Engine
    ↓
Execution + Integration Layer
    ↓
Event Ledger (Canonical Truth)
    ↓
Replay + Review System
```

---

# Architectural Style

TradeForge follows:

- Modular Monolith architecture
- Event-sourced system design
- CQRS-inspired separation (conceptual, not over-engineered)
- Workflow-driven state machine design
- Read-model projections for UI and analytics
- Strict separation of canonical vs derived state

NOT:
- microservices system
- CRUD-centric application
- real-time autonomous trading system
- event-streaming-first distributed system (premature)

---

# Core Bounded Contexts

## 1. Personas Context

Responsible for:
- defining operator behavior model
- shaping workflow interpretation
- controlling relevance of information
- determining playbook applicability

Personas are NOT users — they are operational modes of reasoning.

---

## 2. Workspaces Context

Responsible for:
- persona-scoped operational environments
- decision surfaces
- workflow visibility
- situational awareness structuring

Workspaces are NOT UI containers — they are cognitive operating environments.

---

## 3. Market Intelligence Context

Responsible for:
- interpreting raw market data
- producing contextual understanding
- identifying regimes and conditions
- summarizing macro + micro environments

This layer does NOT:
- generate trade decisions
- mutate workflow state
- define canonical truth

It produces interpreted context.

---

## 4. Scenario Discovery Context

Responsible for:
- generating candidate scenarios
- ranking opportunities and risks
- surfacing watchlist changes
- clustering signals into structured hypotheses

Scenario Discovery is advisory only.

It does NOT:
- own lifecycle state
- execute decisions
- modify ledger events

---

## 5. Decision Lifecycle Context

This is the **core workflow engine**.

Responsible for:
- lifecycle state transitions
- workflow enforcement
- decision queue management
- rule evaluation integration
- approval gates
- invalid state detection

This is the authoritative workflow state machine.

---

## 6. Execution + Integration Context

Responsible for:
- broker communication
- order submission
- external API integration
- market data ingestion
- execution feedback loops

This layer is NOT canonical truth.

External systems are always considered:
- volatile
- external state
- non-authoritative

---

## 7. Event Ledger Context

Responsible for:
- immutable event storage
- canonical truth representation
- audit trail
- workflow reconstruction source

The Event Ledger is the **source of truth for all system state**.

It is:
- append-only
- immutable
- replayable

---

## 8. Replay + Review Context

Responsible for:
- reconstructing historical system state
- replaying decisions and workflows
- supporting reflection and learning
- generating review artifacts

Replay is deterministic and depends only on:
- event ledger
- deterministic rules
- historical snapshots

---

# Data Flow Model

## Write Path

```text
User Intent
    ↓
Decision Lifecycle Engine
    ↓
Event Ledger (append-only)
    ↓
Projections updated asynchronously
```

---

## Read Path

```text
Event Ledger
    ↓
Projections / Read Models
    ↓
Market Intelligence views
    ↓
Workspace surfaces (UI)
```

---

## AI Path (Advisory Only)

```text
Market Data + Context
    ↓
Market Intelligence Layer
    ↓
Scenario Discovery Engine
    ↓
Candidate Scenarios
    ↓
Decision Lifecycle Engine (human-controlled gate)
```

AI NEVER writes to the Event Ledger directly.

---

# State Model Philosophy

TradeForge separates state into:

## 1. Canonical State
- Event Ledger
- Lifecycle transitions
- Workflow facts
- Execution records

## 2. Derived State
- dashboards
- rankings
- summaries
- projections
- UI surfaces

## 3. Inferred State
- market regimes
- AI clustering
- scenario probabilities

Only Canonical State is authoritative.

---
# Knowledge Architecture Doctrine

TradeForge follows a progressive discovery knowledge model.

The knowledge-base is NOT intended to behave as:

* static documentation
* monolithic specifications
* linear design records
* disconnected markdown notes

Instead, the system is designed as an evolving semantic graph composed of:

* ontology nodes
* workflow definitions
* ADRs
* entities
* topics
* replay artifacts
* synthesized operational knowledge

---

# Progressive Discovery Principles

Knowledge should evolve through progressive refinement:

```
raw discovery
    ↓
processed synthesis
    ↓
topic refinement
    ↓
canonical entity definition
    ↓
architectural stabilization
```

The system prioritizes:

* incremental refinement
* semantic linking
* durable relationships
* contextual discovery
* replayable reasoning
* ontology stability

---

# Knowledge Maturity Layers

TradeForge distinguishes between multiple levels of semantic maturity.

## raw/

Exploratory, unstable, imported, or unverified material.

Not canonical.

---

## processed/

Refined synthesis derived from exploratory material.

More stable, but still evolving.

---

## topics/

Cross-cutting thematic syntheses and operational concepts.

Represents higher-level contextual understanding.

---

## entities/

Canonical operational concepts and semantic objects.

These represent stable system meaning.

---

## outputs/

Generated or synthesized artifacts.

Outputs are NOT automatically canonical truth.

Generated material must be explicitly promoted before becoming authoritative.

---

# Semantic Linking

TradeForge uses graph-oriented semantic discovery.

Preferred conventions:

* YAML front matter
* Obsidian-compatible wikilinks
* small semantic nodes
* explicit relationships
* progressive refinement over monolithic documentation

Examples:

```
[[TradePlan]]
[[Replay Session]]
[[Decision Lifecycle]]
[[Market Intelligence Layer]]
```

---

# Architectural Goal

The knowledge-base exists to support:

* replayable reasoning
* ontology stability
* AI-assisted engineering
* semantic governance
* workflow continuity
* long-lived architectural coherence

Correct semantic evolution is more important than rapid documentation expansion.
---

# Event Sourcing Model

All meaningful system changes are represented as events.

Examples:

- `PersonaSelected`
- `ThesisCreated`
- `PlanApproved`
- `OrderSubmitted`
- `FillReceived`
- `PositionClosed`
- `ReviewCompleted`

Events are:
- immutable
- timestamped
- replayable
- versioned if necessary

---

# Workflow Model

The Decision Lifecycle Engine enforces a state machine:

Typical lifecycle:

```text
Idea → Thesis → Plan → Approval → Execution → Position → Review
```

Rules:
- transitions must be explicit
- invalid transitions are rejected
- all transitions generate events
- no hidden state changes allowed

---

# AI Integration Architecture

AI systems operate in:

## Advisory Layer Only

They can:
- generate scenarios
- summarize context
- rank opportunities
- assist review
- highlight anomalies

They cannot:
- bypass workflow engine
- write ledger events
- approve decisions
- mutate lifecycle state

AI is a **context amplifier**, not a system controller.

---

# UX Architecture Mapping

UI is a projection of workspace state.

Core surfaces:

- Briefing Surface (situational awareness)
- Opportunity Surface (scenario discovery output)
- Exposure Surface (active positions + risk)
- Decision Queue (required actions)
- Review Surface (replay + reflection)

UI is NOT navigation-first.

UI is:
- workflow-first
- context-first
- decision-first

---

# Performance Philosophy

TradeForge prioritizes:

- correctness over speed (at architecture level)
- clarity over abstraction
- deterministic behavior over heuristics
- replayability over optimization

Performance optimizations are implemented at projection/read-model level.

---

# System Boundaries

Strict boundaries:

| Layer | Responsibility |
|------|------|
| Personas | behavioral context |
| Workspaces | operational cognition |
| Market Intelligence | interpreted context |
| Scenario Engine | candidate generation |
| Lifecycle Engine | workflow authority |
| Event Ledger | canonical truth |
| Replay System | reconstruction |

No cross-boundary ownership violations allowed.

---

# Design Principle Summary

TradeForge is:

- event-sourced
- workflow-centric
- persona-driven
- replay-first
- AI-assisted but human-controlled
- UX-architected (not UI-added later)

---

# Final Principle

Architecture exists to ensure:

- every decision is traceable
- every workflow is reconstructable
- every state change is explicit
- every AI output is accountable
- every operator retains sovereignty over decisions