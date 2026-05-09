# Repository Guidelines

## Purpose of This Repository

This repository is the canonical knowledge base for the TradeForge system.

TradeForge is a persona-driven, workflow- and decision-centric situational awareness and decision support platform for investing and trading workflows.

This repository is NOT the runtime application codebase.

Its purpose is to preserve and evolve:

- architecture doctrine
- domain ontology
- workflow semantics
- UX philosophy
- event sourcing principles
- replay/review philosophy
- persona and workspace models
- canonical terminology
- architectural decision history
- durable project memory

The linked runtime repository contains implementation details and executable code.

This repository exists to preserve:
- why the system exists
- what concepts mean
- what architectural boundaries matter
- how the system should evolve over time

---
# Mandatory Semantic Boot Sequence

Before performing design or implementation work, Codex MUST initialize semantic context in this order:

## 1. Semantic Truth Layer

Read:

- knowledge-base/TradeForge/SEMANTIC_BOOTSTRAP.md
- knowledge-base/TradeForge/SEMANTIC_GOVERNANCE.md
- knowledge-base/TradeForge/INVARIANTS.md
- knowledge-base/TradeForge/ARCHITECTURE.md
- knowledge-base/TradeForge/UX_DOCTRINE.md
- knowledge-base/TradeForge/GLOSSARY.md
- knowledge-base/TradeForge/EVENT_TAXONOMY.md

Optional depending on task:
- PERSONAS.md
- WORKSPACES.md
- EXECUTION_CONTRACT.md

## 2. Runtime Documentation Layer

Read:
- TradeForge/DOCS/
- TradeForge/DOCS/adr/

## 3. Runtime Implementation Layer

Only after semantic initialization:
- inspect src/
- evaluate implementation impact
- generate design reasoning
- produce code

If semantic initialization has not occurred:
STOP.

# Canonical Truth Hierarchy

When contradictions exist, resolve authority in this order:

1. `INVARIANTS.md`
2. canonical ontology/domain pages
3. ADRs
4. architecture doctrine documents
5. processed knowledge pages
6. raw notes/imports
7. generated outputs

Do NOT silently overwrite contradictions.

Call out inconsistencies explicitly.

---

# Core System Philosophy

TradeForge is:

- persona-driven
- workflow-centric
- decision-centric
- event-sourced
- replay-oriented
- review-oriented
- AI-assisted
- human-governed

TradeForge is NOT:

- an autonomous trading system
- a signal-selling platform
- a CRUD trade tracker
- a generic brokerage dashboard
- an AI-controlled execution engine

The system exists to improve:

- situational awareness
- decision quality
- execution discipline
- reflective learning
- long-term adaptation

---

# Architecture Doctrine

TradeForge is organized around the following conceptual layers:

```text
Personas
    ↓
Persona Workspaces
    ↓
Market Intelligence Layer
    ↓
Scenario Discovery Layer
    ↓
Decision Lifecycle Engine
    ↓
Execution + Integration Layer
    ↓
Event Ledger
    ↓
Replay + Review System
```

Core architectural principles:

- personas shape operational context
- workspaces are operational environments, not app tabs
- market intelligence provides interpreted context, not raw data
- scenario discovery surfaces opportunities and risks
- the lifecycle engine owns workflow state
- the event ledger is canonical truth
- replay/review is a first-class capability
- AI is advisory, never authoritative

---

# UI / UX Doctrine

UI and UX are core architectural concerns.

TradeForge is NOT dashboard-centric.

The frontend is composed of persona-driven operational workspaces.

Each workspace exists to support:

- situational awareness
- workflow continuity
- decision prioritization
- operational clarity
- reflection and review

Canonical decision surfaces include:

- briefing surfaces
- active exposure surfaces
- opportunity/watch surfaces
- decision queues
- review surfaces

Design philosophy:

- context before action
- workflows over navigation
- reduce cognitive fragmentation
- surface operationally relevant information
- preserve continuity of thought
- reflection is part of execution
- uncertainty should be explicit

Avoid generic enterprise-dashboard thinking.

---

# AI Governance Doctrine

AI systems within TradeForge are advisory systems.

AI may:

- summarize
- rank
- cluster
- contextualize
- surface scenarios
- assist review and reflection

AI may NOT:

- mutate canonical lifecycle state
- approve plans
- execute trades
- bypass workflow controls
- write immutable ledger events
- become source-of-truth authority

Human decision sovereignty is a core architectural principle.

---

# Replayability Doctrine

Replayability is foundational.

The system must support reconstruction of:

- visible context
- market conditions
- active positions
- decision queues
- candidate scenarios
- workflow state
- rule evaluations
- reviews and annotations

Replay reconstruction should rely on:

- immutable events
- deterministic rules
- historical snapshots

Replay systems must NOT depend on live external APIs.

---
# Repository Structure

## Core Files

- `PROJECT.md`
  Active initiative focus and current architectural priorities.

- `SEMANTIC_GOVERNANCE.md`
  Canonical semantic governance, canonical readiness, ontology stabilization, and KB processing discipline.

- `ARCHITECTURE.md`
  System architecture doctrine and layer ownership.

- `INVARIANTS.md`
  Non-negotiable system truths.

- `GLOSSARY.md`
  Canonical terminology and semantic definitions.

- `UX_DOCTRINE.md`
  UX and operational workspace philosophy.

- `EVENT_TAXONOMY.md`
  Canonical event categories and semantics.

---

## Main Directories

### `adr/`

Architecture decision records.

ADRs preserve:
- architectural rationale
- tradeoffs
- rejected alternatives
- system evolution history

Use lightweight Markdown ADRs with stable identifiers.

---

### `ontology/`

Canonical concept definitions.

Examples:
- personas
- lifecycle states
- workspace semantics
- review artifacts
- scenarios
- market regimes

Ontology pages are durable semantic references.

---

### `workflows/`

Workflow definitions and operational lifecycle documentation.

Examples:
- thesis lifecycle
- review lifecycle
- approval workflow
- replay workflow

---

### `knowledge/raw/`

Unprocessed notes, imports, transcripts, temporary synthesis, exploratory thinking, and working context.

Raw notes are NOT canonical truth.

---

### `knowledge/processed/`

Refined knowledge synthesized from raw notes and ADRs.

---

### `knowledge/entities/`

Canonical entity and concept pages.

---

### `knowledge/topics/`

Topic syntheses and thematic overviews.

---

### `knowledge/index/`

Navigation and entrypoint pages.

---

### `knowledge/outputs/`

Generated outputs, temporary synthesis artifacts, and AI-generated summaries.

Do NOT treat outputs as canonical without explicit promotion.

---

### `prompts/`

Reusable prompts and restart context for AI-assisted sessions.

---

### `skills/`

Repo-local skills and specialized operational guidance.

Skills are local project artifacts.

Do NOT install them globally unless explicitly requested.

---

# Terminology Discipline

Use canonical terminology consistently.

Preferred terms:

- Persona Workspace
- Decision Surface
- Event Ledger
- Scenario Discovery
- Decision Lifecycle Engine
- Replay Session
- Market Intelligence Layer
- Review Artifact
- Operational Workspace

Avoid introducing alternate terminology unless intentionally evolving the ontology.

When terminology evolves:
- document the change
- preserve historical references
- note deprecated terminology

---

# Markdown Conventions

This repository is Markdown-first.

Guidelines:

- prefer concise sections
- use clear headings
- preserve readability
- prefer ASCII-only diagrams
- use fenced code blocks for structures
- prefer `[[wikilinks]]` for durable internal references

Recommended front matter:

```yaml
---
title: Example Title
type: note
status: draft
tags: [TradeForge]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```
---
# Progressive Discovery Rules

TradeForge follows a progressive discovery knowledge model.

Agents should prefer:

* smaller semantic nodes
* incremental refinement
* explicit conceptual relationships
* cross-linked knowledge
* progressive synthesis over monolithic documents

Prefer:

* YAML front matter
* Obsidian-compatible wikilinks
* durable semantic references
* focused conceptual pages
* explicit ontology relationships

Examples:

```text
[[TradePlan]]
[[Replay Session]]
[[Decision Lifecycle]]
[[Market Intelligence Layer]]
```

Avoid:

* giant master documents
* duplicated semantic definitions
* disconnected notes
* silent ontology drift
* prematurely canonicalizing exploratory material

Knowledge should evolve progressively:

```text
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

Generated outputs are NOT automatically canonical truth.

Promote knowledge incrementally and explicitly.

---

# Knowledge Management Rules

Treat this repository as persistent architectural memory.

When adding or refining knowledge:

- preserve historical context
- synthesize instead of duplicating
- cross-link related concepts
- preserve rejected alternatives where useful
- surface contradictions explicitly
- avoid silent deletion of prior reasoning

Do NOT prematurely canonicalize exploratory notes.

Promote knowledge incrementally.

---

# Linked Runtime Repository

The active runtime implementation repository is expected at:

```text
C:\Users\bosto\dockerstuff\TradeForge\
```

This knowledge base preserves the why behind that implementation:

- architecture rationale
- workflow semantics
- source-of-truth boundaries
- replay philosophy
- UX doctrine
- domain ontology
- decision-system philosophy

The runtime repository should maintain its own implementation-focused `AGENTS.md`.

This repository should remain focused on:

- durable knowledge
- architecture doctrine
- ontology
- workflow semantics
- AI alignment
- UX philosophy
- project memory

---

# Runtime Repository Alignment

Before promoting knowledge to canonical status, verify alignment with:

```text
C:\Users\bosto\dockerstuff\TradeForge\
```

Especially:

- README
- architecture docs
- ADRs
- canonical entity definitions
- lifecycle semantics
- event models

If contradictions exist:
- call them out explicitly
- do not silently replace prior knowledge

---

# Useful Local Commands (PowerShell)

List all Markdown files:

```powershell
Get-ChildItem -Recurse -Filter *.md
```

Search repository for a term:

```powershell
Get-ChildItem -Recurse -File | Select-String "term"
```

Search ontology or architecture concepts:

```powershell
Get-ChildItem ontology -Recurse -File | Select-String "Persona"
```

Read a file:

```powershell
Get-Content path\to\file.md
```

List ADRs:

```powershell
Get-ChildItem adr
```

---

# Agent Instructions

Agents operating in this repository must:

- prioritize architectural consistency
- preserve terminology discipline
- maintain ontology integrity
- treat UX as architecture
- preserve replayability principles
- maintain AI governance boundaries
- prefer clarity over cleverness
- preserve cross-links and historical context

Agents should optimize for:

- conceptual clarity
- durable knowledge
- auditability
- maintainability
- cognitive coherence
- workflow integrity

If uncertain:
- choose the simpler interpretation
- preserve architectural boundaries
- avoid inventing new abstractions
- avoid introducing terminology drift

---

# Final Principle

TradeForge exists to improve disciplined decision-making under uncertainty.

Every addition to this repository should support:

- clarity of thought
- situational awareness
- workflow integrity
- reflective learning
- long-term adaptation
- architectural coherence
- human decision sovereignty
