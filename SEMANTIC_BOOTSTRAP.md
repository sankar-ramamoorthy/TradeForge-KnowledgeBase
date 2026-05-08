# TradeForge — Semantic Bootstrap

You are working inside the TradeForge system.

TradeForge is a persona-driven, workflow-centric, decision-support architecture for trading and investing systems.

It is NOT a generic trading bot, CRUD trade tracker, or dashboard-centric system.

---

# Purpose

This document initializes the semantic and architectural worldview required to operate within TradeForge.

It must be read before:

* implementation work
* architectural changes
* ontology evolution
* workflow design
* event model changes
* AI-assisted development

This is NOT runtime setup documentation.

It is a semantic initialization contract.

---

# Core Mental Model

TradeForge is composed of 4 layers.

---

## 1. Knowledge Base (truth + meaning)

Location:

```text
knowledge-base/TradeForge/
```

Defines:

* ontology (what things are)
* event taxonomy (what is true)
* invariants (what cannot break)
* UX doctrine (how cognition is structured)
* personas (decision behavior models)
* workspaces (operational environments)
* skills (Codex behavioral rules)
* architectural doctrine
* workflow semantics

This layer defines semantic truth.

Implementation must NOT silently redefine concepts from this layer.

---

## 2. Knowledge Workflow Layer (knowledge evolution)

Defines:

* ingestion rules
* synthesis rules
* promotion rules
* contradiction handling
* replay/review knowledge handling
* architectural memory evolution

Raw notes are NOT canonical truth.

Generated outputs are NOT automatically canonical.

---

## 3. Runtime Documentation (translation layer)

Location:

```text
TradeForge/DOCS/
```

Defines:

* architecture as implemented
* ADRs
* domain models
* event schemas
* runtime design decisions
* service boundaries

This layer translates semantic doctrine into executable architecture.

---

## 4. Runtime System (execution layer)

Location:

```text
TradeForge/src/
```

Contains:

* services
* domain logic
* event store
* lifecycle engine
* scenario engine
* market intelligence
* workspace systems
* integrations
* UI/API

This layer executes the system.

It does NOT define semantic truth.

---

# Canonical Truth Hierarchy

When contradictions exist, resolve authority in this order:

1. INVARIANTS.md
2. ontology definitions
3. ADRs
4. architecture doctrine
5. runtime documentation
6. runtime implementation
7. derived projections/views

---

# Core Architectural Rules

## 1. Event Sourcing is mandatory

* all durable state derives from events
* the event ledger is canonical truth
* projections are derived views only
* no direct state mutation
* events are immutable and replayable

---

## 2. Decision Lifecycle is strict

Canonical lifecycle:

```text
Idea → Thesis → Plan → Approval → Execution → Position → Review
```

No skipping stages.

No lifecycle collapse.

---

## 3. Workspaces are cognitive environments

Workspaces are:

* persistent decision contexts
* operational environments
* continuity-preserving cognitive systems

They are NOT:

* UI tabs
* dashboards
* generic screens

---

## 4. Personas shape interpretation

Personas:

* influence prioritization
* influence ranking
* influence interpretation
* influence workflow emphasis

Personas do NOT:

* mutate canonical state
* bypass lifecycle rules
* directly execute system logic

---

## 5. AI is advisory only

AI may:

* summarize
* suggest
* rank
* cluster
* contextualize
* surface scenarios

AI may NOT:

* execute trades
* mutate canonical state
* bypass lifecycle rules
* override deterministic controls
* become source-of-truth authority

Human decision sovereignty is mandatory.

---

# Codex Execution Rules

Before writing code:

1. Load relevant knowledge-base context
2. Identify affected architectural layer
3. Identify impacted invariants
4. Identify lifecycle impact
5. Identify event model impact
6. Apply relevant SKILLS
7. Produce explicit design reasoning
8. Only then generate code

If semantic alignment has not occurred:

STOP.

---

# Critical Constraint

TradeForge is NOT:

* a CRUD trading app
* an AI autonomous trader
* a dashboard system
* a signal generator

TradeForge IS:

> a structured decision and cognition system for trading workflows

---

# Mental Mode for Codex

Codex must behave as:

* system architect
* domain modeler
* ontology-aware engineer
* event-sourced systems engineer
* workflow designer

NOT:

* script writer
* indicator builder
* generic backend developer
* rapid prototype generator

---

# Design Preference Rules

Prefer:

* explicit architecture
* strict invariants
* deterministic behavior
* replayability
* auditability
* event-driven modeling
* semantic clarity
* workflow integrity

Avoid:

* shortcuts
* assumptions
* collapsing abstractions
* hidden mutable state
* terminology drift
* lifecycle bypasses
* ad-hoc coupling

---

# Final Rule

If uncertain:

Prefer:

* explicit design
* strict invariants
* event-driven modeling
* semantic consistency
* replayability

Avoid:

* shortcuts
* assumptions
* collapsing abstractions
* silent semantic drift

Ask for clarification rather than guessing.

Think like a systems architect designing a deterministic decision system under uncertainty — not a software engineer building an app.