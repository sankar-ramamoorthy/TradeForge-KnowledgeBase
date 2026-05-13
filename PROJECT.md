# TradeForge — Project Context

## Current System Identity

Canonical repository name:

```text
TradeForge-KnowledgeBase
```

TradeForge is a persona-driven, workflow- and decision-centric situational awareness and decision support system for investing and trading workflows.

The system is designed to improve:

- situational awareness
- decision quality
- execution discipline
- reflective learning
- long-term behavioral adaptation

TradeForge is NOT:

- an autonomous trading system
- a signal-selling platform
- a generic brokerage dashboard
- a CRUD trade tracker
- an AI-controlled execution engine

The system is designed around:
- human decision sovereignty
- workflow integrity
- replayability
- operational clarity
- context-aware decision support

---

# Vision

TradeForge aims to function as an operational decision environment rather than a traditional trading application.

The platform is centered around persona-driven workspaces that combine:

- interpreted market context
- scenario discovery
- workflow governance
- execution awareness
- replay and review

The goal is to create a system that helps operators make better decisions under uncertainty while continuously improving through structured reflection and replay.

---

# Architectural Direction

The current architectural direction is:

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
- workspaces are operational environments
- workflow state is governed centrally
- the event ledger is canonical truth
- replayability is foundational
- AI systems are advisory only
- UX is a first-class architectural concern

The architecture is intentionally:
- workflow-centric
- event-oriented
- replay-oriented
- modular
- AI-assisted but human-governed

---

# Current Priorities

Current design focus areas:

1. M8 knowledge processing and milestone closeout
2. Persona workspace doctrine refinement after full runtime validation
3. Replay and review semantics now that the first operational slice is complete
4. Post-MVP roadmap checkpoint discipline for M9+ work
5. Canonical ontology and terminology stability
6. Knowledge-base structure and retrieval quality

The current phase is post-M8 knowledge synchronization and roadmap checkpointing after the first operational MVP vertical slice.

---

# Current Non-Goals

The following are intentionally deferred or deprioritized:

- autonomous execution
- high-frequency trading
- generalized social features
- microservices decomposition
- broad plugin architectures
- premature optimization
- AI-controlled workflows
- excessive abstraction
- dashboard-centric UX

The current emphasis is conceptual clarity and architectural coherence.

---

# Active Design Questions

Open architectural questions currently include:

- precise boundaries between Market Intelligence and Scenario Discovery
- event categorization and projection strategy
- replay reconstruction semantics
- workspace composition patterns
- lifecycle state modeling
- historical snapshot strategy
- review workflow structure
- multi-persona coordination
- frontend information hierarchy
- AI provenance and explainability models

Contradictions and unresolved design tensions should be documented explicitly rather than silently resolved.

---

# Immediate Next Steps

Near-term priorities:

1. Finalize core doctrine documents
2. Define canonical terminology
3. Formalize system invariants
4. Define persona models and workspace semantics
5. Define lifecycle states and transitions
6. Define event taxonomy
7. Define replay and review architecture
8. Establish UX and decision-surface doctrine
9. Begin ADR creation
10. Prepare runtime repository architecture

Implementation should follow doctrine, not precede it.

---

# Key Reference Documents

Core references:

- `AGENTS.md`
- `SEMANTIC_GOVERNANCE.md`
- `ARCHITECTURE.md`
- `INVARIANTS.md`
- `UX_DOCTRINE.md`
- `GLOSSARY.md`
- `EVENT_TAXONOMY.md`
- `PERSONAS.md`
- `WORKSPACES.md`

Additional durable knowledge should be promoted incrementally into ontology, workflow, and ADR documents.

---

# Design Philosophy

TradeForge is fundamentally a system for disciplined decision-making under uncertainty.

The platform prioritizes:

- clarity over complexity
- workflows over dashboards
- context over noise
- reflection over activity
- replayability over opacity
- operational awareness over raw data density

Every architectural decision should reinforce:

- human judgment
- auditability
- situational awareness
- workflow integrity
- long-term maintainability
- reflective learning
