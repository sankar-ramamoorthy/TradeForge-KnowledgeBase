# TradeForge — System Invariants

## Purpose

This document defines the non-negotiable architectural and conceptual truths of the TradeForge system.

Invariants are foundational constraints that guide:

- architecture
- workflow design
- event modeling
- replay semantics
- UX philosophy
- AI boundaries
- implementation decisions

When uncertainty exists, invariants take precedence over convenience.

---

# Human Decision Sovereignty

Human operators remain responsible for all material decisions.

The system may:
- assist
- summarize
- rank
- contextualize
- surface opportunities
- highlight risks

The system may NOT:
- autonomously execute trades
- autonomously approve workflows
- silently mutate lifecycle state
- bypass workflow controls
- become authoritative over operator intent

AI systems are advisory only.

---

# Persona-Scoped Operation

All workflows occur within persona context.

Every operational workflow must be associated with:
- a persona
- a workspace context
- temporal context

Personas shape:
- workflow visibility
- prioritization
- playbook relevance
- decision cadence
- review cadence
- operational interpretation

The system is persona-driven, not globally uniform.

---

# Workspaces Are Operational Environments

Workspaces are not generic application tabs.

A workspace is a persona-scoped operational decision environment.

Every workspace should support:
- situational awareness
- workflow continuity
- operational clarity
- decision prioritization
- reflective review

Navigation must not fragment operational cognition unnecessarily.

---

# Workflow-Centric Architecture

TradeForge is workflow-centric, not CRUD-centric.

The system exists to support:
- decision workflows
- lifecycle transitions
- operational awareness
- review and adaptation

The architecture should optimize for:
- clarity of workflow state
- decision continuity
- reconstructability
- operational context

Not:
- generic entity management
- isolated CRUD interactions
- disconnected dashboards

---

# Event Ledger Canonical Truth

The Event Ledger is the canonical source of truth.

Canonical truth resides in:
- immutable events
- lifecycle transitions
- explicit workflow actions

Derived views, projections, dashboards, and summaries are NOT canonical truth.

Broker APIs and external systems are NOT canonical truth.

---

# Events Are Immutable

Historical events must never be mutated or deleted.

Events represent facts that occurred.

Events should:
- be append-only
- preserve temporal ordering
- remain auditably reconstructable
- support replay and review

Corrections must occur through new events, not mutation.

---

# Replayability Is Foundational

All material workflows must support replay reconstruction.

Replay should reconstruct:
- visible context
- operational state
- active workflows
- decision queues
- scenarios
- market conditions
- reviews and annotations

Replay systems should rely only on:
- immutable events
- deterministic rules
- historical snapshots

Replay must not depend on live APIs or mutable external state.

---

# Lifecycle Authority

The Decision Lifecycle Engine owns workflow state.

Lifecycle state transitions must:
- be explicit
- be auditable
- be event-backed
- follow deterministic rules

No external subsystem may silently mutate lifecycle state.

Scenario discovery systems, AI systems, and UI systems do NOT own lifecycle authority.

---

# AI Advisory Boundary

AI systems are advisory systems.

AI-generated outputs are:
- suggestions
- summaries
- rankings
- contextual artifacts

AI outputs are NOT canonical truth.

AI systems must not:
- write immutable ledger events directly
- autonomously transition lifecycle state
- bypass workflow rules
- conceal uncertainty
- obscure provenance

AI-generated artifacts should preserve:
- provenance
- explainability where practical
- operator editability

---

# Market Intelligence Is Interpreted Context

The Market Intelligence Layer provides interpreted situational context.

It is NOT merely raw data ingestion.

Market intelligence may include:
- market regimes
- macro context
- volatility conditions
- breadth conditions
- thematic narratives
- playbook activation conditions

Interpretations must remain separable from canonical historical facts.

---

# Scenario Discovery Is Non-Authoritative

Scenario Discovery systems surface:
- opportunities
- risks
- candidate setups
- watchlist changes
- anomalies
- ranked scenarios

Scenario Discovery systems do NOT:
- own workflow state
- own execution authority
- own lifecycle transitions
- become canonical truth

Scenario Discovery informs decisions.
It does not govern them.

---

# UX Is Architectural

UX is not a cosmetic layer.

UX directly affects:
- decision quality
- situational awareness
- operational clarity
- cognitive load
- workflow continuity

Interfaces should prioritize:
- context before action
- workflow continuity
- operational relevance
- cognitive clarity
- information hierarchy
- explicit uncertainty

Avoid:
- dashboard sprawl
- unnecessary navigation
- fragmented workflows
- information overload

---

# Reflection And Review Are First-Class

Review and reflection are core workflows.

The system should support:
- post-decision analysis
- replay sessions
- behavioral review
- rule evaluation
- adaptation workflows

Review artifacts are durable operational knowledge.

The platform is designed for long-term operator improvement, not only execution support.

---

# Deterministic Rule Evaluation

Rules should be:
- deterministic
- explicit
- auditable
- replayable

Rule evaluation outcomes should be reconstructable from:
- historical events
- deterministic logic
- historical context

Hidden heuristics should be avoided in workflow-critical systems.

---

# Derived State Must Remain Distinguishable

The system must clearly distinguish between:
- canonical state
- derived state
- inferred state
- AI-generated interpretation

Examples:

Canonical:
- lifecycle events
- approvals
- fills
- reviews

Derived:
- dashboards
- rankings
- summaries
- projections
- watchlists

Inferred:
- market regime classifications
- AI confidence estimates
- thematic clustering

Users and systems should not confuse derived interpretation with historical fact.

---

# Architectural Simplicity

Prefer:
- explicit systems
- understandable workflows
- clear ownership boundaries
- deterministic behavior
- maintainable structures

Avoid:
- premature abstraction
- unnecessary indirection
- speculative infrastructure
- generalized frameworks without demonstrated need

Complexity must justify itself operationally.

---

# Modularity Over Fragmentation

TradeForge is conceptually modular but operationally cohesive.

The architecture should preserve:
- bounded responsibilities
- clear ownership
- separable concerns

Without introducing:
- unnecessary distributed systems complexity
- premature microservices decomposition
- fragmented workflow ownership

The system should behave as a coherent operational environment.

---

# Terminology Stability

Canonical terminology matters.

Terminology should remain:
- explicit
- stable
- semantically consistent

Changes to terminology should:
- be deliberate
- be documented
- preserve historical understanding

Avoid unnecessary synonym drift.

---

# Historical Integrity

Historical operational context should be preserved whenever practical.

The system should support understanding:
- what was known
- what was visible
- what alternatives existed
- why decisions were made

Historical reconstruction is a core capability, not an optional analytics feature.

---

# Final Invariant

TradeForge exists to improve disciplined decision-making under uncertainty.

Every architectural, UX, workflow, and implementation decision should reinforce:

- clarity
- awareness
- auditability
- reflection
- workflow integrity
- operational coherence
- human judgment
- long-term learning