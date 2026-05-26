# TradeForge — Knowledge Base
[![Ask DeepWiki](https://deepwiki.com/badge.svg)](https://deepwiki.com/sankar-ramamoorthy/TradeForge-KnowledgeBase)

> **Work in progress.** This is the semantic and architectural governance repository for TradeForge.

This repository is the canonical knowledge system for [TradeForge](https://github.com/sankar-ramamoorthy/TradeForge) — a structured cognition and decision system for discretionary trading workflows.

It is **not** the executable application. The running system lives in the runtime repository:

👉 **[TradeForge Runtime](https://github.com/sankar-ramamoorthy/TradeForge)**

---

## What Is This Repository?

TradeForge treats its own development as a replayable cognition system. This repository is the memory and governance layer that makes that possible.

It preserves:

* **Ontology** — what things mean in TradeForge (decisions, workspaces, personas, events, replays)
* **Invariants** — architectural truths that must never break
* **Architectural doctrine** — why the system is built the way it is
* **Workflow semantics** — how workflows behave and why
* **UX philosophy** — cognition-first, anti-dashboard interaction principles
* **AI governance rules** — what AI may and may not do inside the system
* **Replay philosophy** — how historical reconstruction should work
* **Development memory** — planning captures, implementation notes, processed knowledge

The separation exists by design. Code evolves quickly. Semantic meaning and architectural rationale should evolve deliberately. This repository is the deliberate layer.

---

## Why a Separate Repository?

Most projects embed architectural intent in code comments, Confluence pages, or ADRs that drift from reality. TradeForge takes a different approach:

* The **knowledge base** defines what is true semantically and architecturally
* The **runtime repository** implements that truth in code
* AI-assisted development sessions boot from this repository before writing any code
* Planning, implementation notes, and architectural observations are captured here and promoted through a knowledge stabilization process

This means:

* Future development sessions — including AI-assisted ones — can reconstruct architectural intent without reading code history
* Architectural drift is detectable and correctable by comparing the knowledge base against the implementation
* Semantic meaning is preserved even as implementation details change

---

## Repository Structure

### Root doctrine files

| File | Purpose |
|---|---|
| `INVARIANTS.md` | Non-negotiable architectural and semantic truths |
| `SEMANTIC_BOOTSTRAP.md` | System worldview and operational initialization for AI-assisted development |
| `SEMANTIC_GOVERNANCE.md` | Rules for how knowledge stabilizes, promotes, and governs doctrine |
| `ARCHITECTURE.md` | Canonical architecture doctrine and layer ownership |
| `GLOSSARY.md` | Canonical terminology and semantic definitions |
| `UX_DOCTRINE.md` | Operational workspace and cognition philosophy |
| `EVENT_TAXONOMY.md` | Canonical event semantics and classifications |
| `EXECUTION_CONTRACT.md` | Rules for how AI-assisted development sessions must operate |

---

### `knowledge/`

The progressive knowledge stabilization layer.

```
knowledge/
├── index/        Navigation, semantic entrypoints, context map
├── raw/          Unprocessed planning and implementation captures
├── processed/    Refined and synthesized knowledge
├── entities/     Canonical entity definitions (TradeIdea, ReplaySession, etc.)
├── topics/       Thematic syntheses (replayability, AI advisory, etc.)
└── outputs/      Temporary synthesis artifacts
```

Knowledge flows: `raw → processed → entities/topics` through deliberate promotion. Raw captures are not canonical. Entities are.

---

### `playbooks/`

Operational governance systems and repeatable development workflows.

Key playbook: `playbooks/development/runtime-kb-development-loop.md`

This defines the canonical development workflow — how runtime implementation and knowledge base work stay synchronized across development sessions.

---

### `prompts/`

Reusable AI-assisted workflow entry points.

```
prompts/workflow/
├── runtime-planning.md      Planning phase prompt
├── runtime-implementation.md  Implementation phase prompt
├── kb-processing.md         Knowledge stabilization prompt
└── operational-sync.md      Roadmap/register sync prompt
```

These prompts operationalize the development loop in a reproducible way.

---

### `skills/`

Behavioral reasoning constraints for AI-assisted sessions.

Skills define *how* to think — event sourcing discipline, lifecycle enforcement, replay integrity, workspace cognition. They are not procedures; they are reasoning constraints applied during development.

---

### `ontology/`

Canonical concept definitions — personas, lifecycle states, workspace semantics, market regimes, scenario structures, review artifacts.

---

## Canonical Truth Hierarchy

When contradictions exist, authority resolves in this order:

1. `INVARIANTS.md`
2. Ontology definitions
3. ADRs (in the runtime repository)
4. Architecture doctrine
5. Processed knowledge
6. Raw notes and captures

Implementation must never silently redefine semantic meaning.

---

## Core Invariants (Summary)

* **Human decision sovereignty** — the system assists, never decides
* **Event ledger is canonical truth** — all durable state derives from immutable events
* **Replayability is foundational** — all material workflows must support deterministic reconstruction
* **AI is advisory only** — AI may summarize, rank, and contextualize; it may never mutate canonical state, execute trades, or bypass lifecycle controls
* **Workflow-centric, not CRUD-centric** — the architecture optimizes for decision workflows, not generic entity management

See `INVARIANTS.md` for the full list.

---

## Relationship to the Runtime

| This repository | Runtime repository |
|---|---|
| Defines what things mean | Executes the system |
| Ontology, doctrine, invariants | Services, events, APIs, UI |
| Governs architecture | Implements architecture |
| Preserved and evolved deliberately | Evolves with implementation |
| [TradeForge-KnowledgeBase](https://github.com/sankar-ramamoorthy/TradeForge-KnowledgeBase) | [TradeForge](https://github.com/sankar-ramamoorthy/TradeForge) |

When contradictions arise between the two repositories, the knowledge base is authoritative for semantic meaning. The runtime is authoritative for what is actually implemented today.

---

## Current Status

The knowledge base is synchronized with the runtime through active **M14 (Behavioral Intelligence and Cognitive Auditability)**. TF-C001 has stabilized the first deterministic `BehavioralSignal` boundary for recurring sizing violations, while TF-C002 through TF-C010 remain planned M14 follow-on work.

Ongoing work: ontology promotion, topic synthesis, playbook evolution as the runtime expands.

---

## Contributing

This is a solo architectural project. The knowledge base is public for transparency — to show how a semantic governance layer can work alongside a runtime codebase, and what AI-assisted architecture discipline looks like in practice.

Questions and observations are welcome via issues on the runtime repository.

---

*TradeForge — structured cognition for discretionary trading.*
