---
id: skill-system-index
title: TradeForge Skills System
category: meta
applies_to:
  - codex
  - claude
version: 1.0
description: Defines the active operational skill system for TradeForge — behavioral reasoning overlays that govern how agents reason about architecture, domain concepts, workflows, and invariants.
---

# TradeForge Skills System

## Purpose

This file defines the active operational skill system for agents within the TradeForge knowledge-base.

Skills are behavioral reasoning overlays.

They modify how Agent:

* reasons about architecture
* interprets domain concepts
* evaluates workflows
* generates or modifies designs
* enforces invariants
* performs event modeling
* evaluates replayability
* preserves semantic consistency

Skills are NOT:

* architectural documentation
* ontology definitions
* runtime implementation guides
* generic knowledge files
* feature specifications

Those belong elsewhere in the knowledge-base.

---

# Core Principle

Skills are:

> context-sensitive reasoning rules that govern how Agent behaves inside TradeForge.

Skills define:

* reasoning constraints
* architectural discipline
* workflow enforcement
* semantic boundaries
* operational interpretation behavior

Skills exist to reduce:

* architectural drift
* ontology drift
* lifecycle violations
* replay inconsistencies
* semantic collapse
* dashboard-oriented thinking
* shortcut-driven implementation

---

# Skills vs Prompts

Skills define:

> how Agent should think.

Prompts define:

> what Agent should do.

Example:

```text
prompts/adr/GENERATE_ADRS.md
```

may invoke:

* architecture/event-sourcing.md
* architecture/decision-lifecycle.md
* architecture/execution-discipline.md

The prompt defines the workflow.

The skills define the reasoning constraints applied during that workflow.

---

# Skill Categories

## architecture/

Core architectural reasoning rules.

These skills enforce:

* event-sourcing integrity
* lifecycle correctness
* replayability
* immutable event semantics
* architectural boundaries
* execution discipline

Examples:

* architecture/event-sourcing.md
* architecture/decision-lifecycle.md
* architecture/execution-discipline.md

---

## cognition/

Operational cognition and interpretation behavior.

These skills govern:

* market interpretation
* contextual prioritization
* scenario evaluation
* workspace reasoning
* operational awareness

Examples:

* cognition/market-interpreting.md
* cognition/scenario-ranking.md
* cognition/workspace-querying.md

---

## ontology/

Ontology and semantic consistency rules.

These skills govern:

* terminology discipline
* semantic consistency
* abstraction boundaries
* concept integrity
* ontology feedback and refinement

Examples:

* ontology/structure-feedback-note.md

---

# Skill Activation Model

When operating inside TradeForge:

1. Load semantic context from:

   * SEMANTIC_BOOTSTRAP.md
   * INVARIANTS.md
   * ARCHITECTURE.md
   * relevant ontology/workflow documents

2. Identify affected architectural or domain area.

3. Activate relevant skills mentally BEFORE generating output.

4. Apply skill constraints during:

   * reasoning
   * design
   * implementation
   * review
   * ADR generation
   * replay analysis

5. Preserve semantic and architectural integrity over convenience.

Multiple skills may be active simultaneously.

---

# Skill Composition

Skills are composable.

Example:

A replay-analysis workflow may simultaneously activate:

* architecture/event-sourcing.md
* architecture/decision-lifecycle.md
* cognition/workspace-querying.md

An ADR-generation workflow may simultaneously activate:

* architecture/event-sourcing.md
* architecture/execution-discipline.md
* ontology/structure-feedback-note.md

Skills should reinforce each other rather than conflict.

---

# Skill Authoring Rules

New skills should:

* solve a specific reasoning problem
* enforce a stable architectural behavior
* preserve semantic consistency
* remain reusable across workflows
* avoid implementation-specific coupling

Skills should NOT:

* duplicate architecture docs
* redefine ontology
* bypass invariants
* introduce new terminology casually
* contain unrelated workflow instructions

When introducing a new skill:

* place it in the correct category
* preserve terminology discipline
* document its purpose clearly
* avoid overlapping responsibilities where possible

---

# Execution Rule

When reasoning about TradeForge:

1. Identify the relevant domain area.
2. Load the corresponding semantic context.
3. Activate the relevant skills mentally.
4. Apply skill constraints BEFORE generating output.
5. Validate against invariants and lifecycle rules.
6. Prefer architectural correctness over convenience.

Never override skill rules merely to accelerate implementation.

---

# Relationship to Runtime Repository

Skills belong to the semantic knowledge system:

```text
knowledge-base/TradeForge/skills/
```

They are consumed by workflows operating inside the runtime repository:

```text
TradeForge/
```

The runtime repository executes the system.

The skills system governs how Agent reasons about that system.

---

# Final Principle

Skills define how Agent behaves inside TradeForge.

Correct reasoning is more important than rapid generation.

If uncertain:

* preserve invariants
* preserve replayability
* preserve lifecycle integrity
* preserve semantic consistency
* ask for clarification rather than guessing
