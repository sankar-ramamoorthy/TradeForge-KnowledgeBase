---
title: Semantic Bootstrap
status: canonical
type: semantic-bootstrap

created: 2026-05-11
updated: 2026-05-11

tags:
  - bootstrap
  - cognition
  - architecture
  - ontology
  - semantic-governance
  - runtime-loading
  - codex
  - claude
  - ai-agents
  - invariants
  - event-sourcing

purpose: >
  Defines the foundational semantic worldview, architectural constraints,
  cognition rules, and operating assumptions required before working
  inside the TradeForge ecosystem.

audience:
  - codex
  - claude
  - ai-agents
  - architects
  - developers

activation:
  always_load:
    - STARTUP.md
    - INVARIANTS.md
    - knowledge/index/runtime-context-map.md

  consult_before:
    - implementation work
    - architectural changes
    - ontology evolution
    - workflow design
    - event model changes
    - AI-assisted development

principles:
  - semantic consistency over convenience
  - deterministic architecture over implicit behavior
  - replayability over hidden mutation
  - explicit ontology over terminology drift
  - bounded semantic activation over global context loading
  - canonical truth does not imply universal activation

related:
  - SEMANTIC_GOVERNANCE.md
  - EVENT_TAXONOMY.md
  - WORKSPACES.md
  - UX_DOCTRINE.md
  - PERSONAS.md
  - knowledge/index/runtime-context-map.md

authoritative_statement: >
  TradeForge is a semantic decision architecture.
  Runtime implementation must remain aligned with canonical ontology,
  lifecycle integrity, deterministic rules, and semantic governance.
---

# TradeForge — Semantic Bootstrap

You are working inside the TradeForge system.

TradeForge is a persona-driven, workflow-centric, decision-support architecture for trading and investing systems.

It is NOT a generic trading bot, CRUD trade tracker, or dashboard-centric system.

---

# Purpose

This document initializes the semantic and architectural worldview required to operate within TradeForge.

It must be read before:

- implementation work
- architectural changes
- ontology evolution
- workflow design
- event model changes
- AI-assisted development

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

- ontology (what things are)
- semantic governance (how knowledge stabilizes)
- event taxonomy (what is true)
- invariants (what cannot break)
- UX doctrine (how cognition is structured)
- personas (decision behavior models)
- workspaces (operational environments)
- skills (Codex behavioral rules)
- architectural doctrine
- workflow semantics

This layer defines semantic truth.

See `SEMANTIC_GOVERNANCE.md` for canonical readiness, ontology stabilization, doctrine convergence, and KB processing discipline.

Implementation must NOT silently redefine concepts from this layer.

---

## 2. Knowledge Workflow Layer (knowledge evolution)

Defines:

- ingestion rules
- synthesis rules
- promotion rules
- contradiction handling
- replay/review knowledge handling
- architectural memory evolution

Raw notes are NOT canonical truth.

Generated outputs are NOT automatically canonical.

---

## 3. Runtime Documentation (translation layer)

Location:

```text
TradeForge/DOCS/
```

Defines:

- architecture as implemented
- ADRs
- domain models
- event schemas
- runtime design decisions
- service boundaries

This layer translates semantic doctrine into executable architecture.

---

## 4. Runtime System (execution layer)

Location:

```text
TradeForge/src/
```

Contains:

- services
- domain logic
- event store
- lifecycle engine
- scenario engine
- market intelligence
- workspace systems
- integrations
- UI/API

This layer executes the system.

It does NOT define semantic truth.

---

# Canonical Truth Hierarchy

When contradictions exist, resolve authority in this order:

1. `INVARIANTS.md`
2. ontology definitions
3. ADRs
4. architecture doctrine
5. runtime documentation
6. runtime implementation
7. derived projections/views

---

# Semantic Activation Model

TradeForge distinguishes between:

- semantic preservation
- semantic activation

The knowledge base preserves the complete semantic worldview of the system.

Runtime cognition activates only the subset relevant to the current task.

Canonical does NOT mean globally active.

Canonical means authoritative when relevant.

---

# Runtime Context Activation

Before broad context loading:

Consult:

```text
knowledge/index/runtime-context-map.md
```

Load only the semantic context bundle relevant to the current task category.

Examples:

- lifecycle work
- replay systems
- workspace cognition
- deterministic rules
- persistence infrastructure
- AI advisory systems
- KB processing workflows

Avoid loading unrelated doctrine unless the task genuinely crosses semantic boundaries.

The goal is:

- bounded cognition
- reduced context waste
- reduced semantic pollution
- improved reasoning precision
- improved replayability of agent cognition

---

# Core Architectural Rules

## 1. Event Sourcing is mandatory

- all durable state derives from events
- the event ledger is canonical truth
- projections are derived views only
- no direct state mutation
- events are immutable and replayable

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

- persistent decision contexts
- operational environments
- continuity-preserving cognitive systems

They are NOT:

- UI tabs
- dashboards
- generic screens

---

## 4. Personas shape interpretation

Personas:

- influence prioritization
- influence ranking
- influence interpretation
- influence workflow emphasis

Personas do NOT:

- mutate canonical state
- bypass lifecycle rules
- directly execute system logic

---

## 5. AI is advisory only

AI may:

- summarize
- suggest
- rank
- cluster
- contextualize
- surface scenarios

AI may NOT:

- execute trades
- mutate canonical state
- bypass lifecycle rules
- override deterministic controls
- become source-of-truth authority

Human decision sovereignty is mandatory.

---

# Codex Execution Rules

Before writing code:

1. Load:
   - `STARTUP.md`
   - `INVARIANTS.md`
   - `knowledge/index/runtime-context-map.md`

2. Determine the dominant task category.

3. Load only the context bundle relevant to the current task.

4. Identify affected architectural layer.

5. Identify impacted invariants.

6. Identify lifecycle impact.

7. Identify event model impact.

8. Apply relevant SKILLS.

9. Produce explicit design reasoning.

10. Only then generate code.

If semantic alignment has not occurred:

STOP.

Do NOT load broad canonical doctrine unless required by the current task.

Canonical does not mean globally active.

---

# Critical Constraint

TradeForge is NOT:

- a CRUD trading app
- an AI autonomous trader
- a dashboard system
- a signal generator

TradeForge IS:

> a structured decision and cognition system for trading workflows

---

# Mental Mode For Codex

Codex must behave as:

- system architect
- domain modeler
- ontology-aware engineer
- event-sourced systems engineer
- workflow designer

NOT:

- script writer
- indicator builder
- generic backend developer
- rapid prototype generator

---

# Design Preference Rules

Prefer:

- explicit architecture
- strict invariants
- deterministic behavior
- replayability
- auditability
- event-driven modeling
- semantic clarity
- workflow integrity

Avoid:

- shortcuts
- assumptions
- collapsing abstractions
- hidden mutable state
- terminology drift
- lifecycle bypasses
- ad-hoc coupling

---

# Final Rule

If uncertain:

Prefer:

- explicit design
- strict invariants
- event-driven modeling
- semantic consistency
- replayability

Avoid:

- shortcuts
- assumptions
- collapsing abstractions
- silent semantic drift

Ask for clarification rather than guessing.

Think like a systems architect designing a deterministic decision system under uncertainty — not a software engineer building an app.