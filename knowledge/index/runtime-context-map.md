

---
title: Runtime Context Map
status: canonical
type: runtime-index
created: 2026-05-11
updated: 2026-05-11
tags:
  - context-management
  - semantic-activation
  - runtime-loading
  - codex
  - claude
  - cognition
  - knowledge-base
  - architecture
  - workflow
purpose: >
  Defines semantic activation boundaries for runtime agent cognition.
  Maps categories of work to the minimal canonical context required
  for effective reasoning and implementation.

principles:
  - Canonical does not mean always active.
  - Load the minimum viable semantic context.
  - Prefer bounded cognition over global awareness.
  - Prevent context-window pollution.
  - Preserve semantic coherence while reducing token waste.
  - Separate preservation from activation.

activation_model:
  preservation_layer:
    description: >
      The complete canonical knowledge system.
      Includes doctrine, ADRs, entities, topics, playbooks,
      glossary terms, workflows, and historical decisions.

  activation_layer:
    description: >
      The dynamically loaded subset of canonical context
      required for the current task.

rules:
  - Always consult this document before broad context loading.
  - Only load contexts relevant to the current work category.
  - Avoid loading unrelated doctrine "just in case".
  - Prefer stable canonical summaries over large raw notes.
  - Escalate context breadth incrementally when necessary.
  - Use entities and topics before loading entire doctrine files.
  - Preserve semantic boundaries between subsystems.

loading_order:
  - STARTUP.md
  - INVARIANTS.md
  - knowledge/index/runtime-context-map.md
  - task-specific context bundle

context_escalation_policy:
  level_1:
    description: Minimal task-local context only.

  level_2:
    description: Related entities and topics.

  level_3:
    description: Relevant doctrine and ADRs.

  level_4:
    description: Cross-domain architecture context.

  level_5:
    description: Full-system reasoning mode. Use sparingly.

anti_patterns:
  - Loading the entire KB for implementation tasks.
  - Treating all canonical doctrine as globally active.
  - Pulling workspace cognition into persistence work.
  - Pulling replay architecture into UI work.
  - Pulling AI strategy context into deterministic rule implementation.
  - Using raw brainstorm notes as runtime cognition defaults.

authoritative_statement: >
  Canonical knowledge is authoritative when relevant,
  not universally active at all times.
---

# Runtime Context Map

# Purpose

This document defines semantic activation boundaries for runtime cognition.

The knowledge base preserves semantic truth across the entire system.

This document determines:

- what should become active
- when it should become active
- how much context should be loaded
- which canonical domains are relevant to a task

The goal is to reduce:

- context-window waste
- semantic dilution
- unnecessary doctrine loading
- cross-domain cognitive pollution

while preserving:

- semantic integrity
- architectural correctness
- canonical consistency
- reasoning quality

---

# Core Principle

> Canonical does not mean always active.
>
> Canonical means authoritative when relevant.

---

# Runtime Loading Model

## Step 1 — Load Global Foundations

Always load:

- `STARTUP.md`
- `INVARIANTS.md`
- `knowledge/index/runtime-context-map.md`

These define:

- global operating constraints
- semantic safety boundaries
- activation discipline

---

## Step 2 — Identify Task Category

Determine the dominant category of work.

Examples:

- lifecycle transitions
- replay systems
- workspaces
- deterministic rules
- persistence
- AI advisory systems
- ontology processing
- KB promotion workflows
- UI projections
- event sourcing
- infrastructure integration

---

## Step 3 — Load Minimal Relevant Context

Load only the context bundle associated with the task category.

Do not load unrelated doctrine preemptively.

Escalate context breadth only if required.

---

# Context Bundles

---

# Lifecycle / State Transition Work

## Use When

Task touches:

- lifecycle transitions
- state changes
- execution workflows
- audit trails
- domain event creation
- transition validation
- position state evolution

## Load

Core doctrine:

- `INVARIANTS.md`

Canonical entities:

- `LifecycleEvent`
- `TradeIdea`
- `TradeThesis`
- `TradePlan`
- `Position`
- `Fill`
- `TradeReview`

Canonical topics:

- execution contract
- lifecycle transitions
- event semantics

Canonical doctrine:

- `EVENT_TAXONOMY.md`

Relevant ADRs:

- event sourcing ADRs
- lifecycle integrity ADRs
- execution workflow ADRs

## Avoid Loading

Unless directly required:

- workspace doctrine
- replay architecture
- persona systems
- AI advisory systems
- projection systems

---

# Replay / Historical Reconstruction Work

## Use When

Task touches:

- replay systems
- historical reconstruction
- timeline rebuilding
- deterministic replay
- simulation playback
- historical analysis
- state reconstruction from events

## Load

Core doctrine:

- `INVARIANTS.md`

Canonical entities:

- `ReplaySession`
- `HistoricalReconstruction`
- `LifecycleEvent`

Canonical topics:

- replay systems
- deterministic reconstruction
- historical simulation

Canonical doctrine:

- `EVENT_TAXONOMY.md`

Relevant ADRs:

- replay architecture ADRs
- event sourcing ADRs

## Avoid Loading

Unless directly required:

- workspace UX doctrine
- UI projections
- execution adapters
- broker integrations
- persona cognition systems

---

# Workspace / Cognitive UX Work

## Use When

Task touches:

- workspaces
- trader cognition
- projections
- workflow environments
- UI state
- operational screens
- interaction flow
- persona-aware interfaces

## Load

Canonical doctrine:

- `UX_DOCTRINE.md`
- `WORKSPACES.md`
- `PERSONAS.md`

Canonical entities:

- `Workspace`
- `Projection`
- `Persona`

Canonical topics:

- workspace cognition
- cognitive UX
- operational flow

Relevant ADRs:

- workspace architecture ADRs
- projection ADRs
- UX doctrine ADRs

## Avoid Loading

Unless directly required:

- replay internals
- persistence infrastructure
- execution reconciliation
- infrastructure adapters
- AI training systems

---

# Deterministic Rule Engine Work

## Use When

Task touches:

- deterministic rules
- validations
- discipline enforcement
- rule evaluation
- violation systems
- approval gates

## Load

Core doctrine:

- `INVARIANTS.md`

Canonical entities:

- `Rule`
- `RuleEvaluation`
- `Violation`

Canonical topics:

- deterministic discipline
- rule enforcement
- auditability

Relevant ADRs:

- rules engine ADRs
- discipline enforcement ADRs

## Avoid Loading

Unless directly required:

- workspace systems
- replay systems
- AI cognition systems
- broker execution infrastructure

---

# Persistence / Infrastructure Work

## Use When

Task touches:

- repositories
- storage adapters
- JSON persistence
- SQLAlchemy
- migrations
- infrastructure boundaries
- serialization
- transaction boundaries

## Load

Canonical doctrine:

- architecture doctrine
- infrastructure boundaries

Canonical entities:

- repository interfaces
- unit of work
- persistence adapters

Canonical topics:

- infrastructure separation
- persistence strategy

Relevant ADRs:

- persistence ADRs
- repository ADRs
- infrastructure ADRs

## Avoid Loading

Unless directly required:

- personas
- workspaces
- replay systems
- AI cognition systems
- trader workflow UX

---

# AI Advisory / Intelligence Systems

## Use When

Task touches:

- AI advisory
- recommendations
- regime analysis
- pattern recognition
- simulations
- RL systems
- adaptive behavior
- strategy intelligence
- provider governance for advisory systems
- AI gateway routing and route aliases
- managed advisory runtime boundaries
- governed downstream provider secrets
- advisory smoke tests and diagnostics

## Load

Canonical doctrine:

- AI doctrine
- advisory boundaries
- managed AI gateway and governed provider-secret doctrine

Canonical entities:

- advisory systems
- simulation entities
- regime models
- provider governance artifacts

Canonical topics:

- adaptive systems
- trading intelligence
- reinforcement learning
- strategy analysis
- provider governance
- AI gateway routing
- operational diagnostics
- credential governance

Relevant ADRs:

- AI architecture ADRs
- RL integration ADRs
- managed AI gateway ADRs
- credential governance ADRs

## Avoid Loading

Unless directly required:

- persistence implementation details
- UI rendering systems
- infrastructure internals

---

# Knowledge Base Processing Work

## Use When

Task touches:

- raw note processing
- ontology promotion
- semantic extraction
- glossary promotion
- topic creation
- doctrine refinement
- KB governance

## Load

Canonical doctrine:

- `SEMANTIC_GOVERNANCE.md`
- `SEMANTIC_BOOTSTRAP.md`

Canonical topics:

- ontology evolution
- semantic governance
- promotion discipline

Canonical entities:

- ontology nodes
- workflow prompts
- Codex skills

Relevant playbooks:

- KB processing workflows
- runtime KB development loop

## Avoid Loading

Unless directly required:

- replay internals
- execution systems
- broker adapters
- trading workflows

---

# Context Escalation Rules

## Rule 1 — Start Narrow

Always begin with the smallest viable semantic bundle.

---

## Rule 2 — Expand Incrementally

Only widen context if reasoning quality suffers.

---

## Rule 3 — Prefer Summaries Over Dumps

Prefer:

- entities
- topics
- ADR summaries

before loading:

- large doctrine files
- raw brainstorm sessions
- broad architecture discussions

---

## Rule 4 — Avoid Cross-Domain Pollution

Do not activate unrelated systems unless the task genuinely crosses boundaries.

---

# Operational Guidance For Agents

When beginning work:

1. Identify dominant task category.
2. Load corresponding context bundle.
3. Avoid unrelated doctrine.
4. Escalate context breadth only if necessary.
5. Preserve semantic boundaries.
6. Keep cognition local whenever possible.

---

# Expected Outcomes

Correct use of this system should:

- reduce token consumption
- improve reasoning focus
- reduce semantic drift
- improve implementation precision
- reduce hallucinated cross-domain coupling
- improve replayability of agent cognition
- improve Codex and Claude session efficiency

---

# Final Principle

The purpose of the knowledge base is not to make every idea globally active.

The purpose of the knowledge base is:

- semantic preservation
- semantic governance
- semantic retrieval
- semantic activation

in the correct scope,
at the correct time,
for the correct task.
