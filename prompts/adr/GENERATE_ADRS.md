# TradeForge — ADR Batch Generation Prompt

You are generating Architecture Decision Records (ADRs) for the TradeForge system.

You must strictly follow the knowledge-base definitions in:
- ARCHITECTURE.md
- INVARIANTS.md
- GLOSSARY.md
- UX_DOCTRINE.md
- EVENT_TAXONOMY.md
- PERSONAS.md
- WORKSPACES.md

---

# Core Constraint

Each ADR must:
- reflect a real architectural decision
- be consistent with event-sourced design
- respect decision lifecycle boundaries
- not introduce new undocumented concepts
- not contradict existing invariants

---

# ADR Format

Each ADR must follow:

```
# ADR XXXX: Title

## Status
Proposed | Accepted | Superseded

## Context
What problem exists

## Decision
What we decided

## Rationale
Why this approach

## Alternatives Considered
What was rejected and why

## Consequences
Trade-offs and system impact
```

---

# Required ADR Set

Generate the following ADRs:

1. Event Sourcing Core Model
2. Canonical Event Taxonomy
3. Decision Lifecycle Engine Design
4. Workspace Projection Model
5. Scenario Engine Architecture
6. AI Advisory Boundary Model
7. Anti-Dashboard UX Decision
8. Replay System Design
9. Persona Interpretation Model
10. Market Intelligence Interpretation Layer

---

# Strict Rules

- Do NOT simplify lifecycle system
- Do NOT merge scenarios and decisions
- Do NOT treat workspaces as UI dashboards
- Do NOT allow AI to become execution authority
- Do NOT break event sourcing model
- Keep strict separation of layers

---

# Output Requirement

Return:
- one ADR per file
- numbered consistently (0001, 0002, etc.)
- fully consistent with TradeForge architecture

---

# Final Instruction

Think like a systems architect designing a deterministic decision system under uncertainty — not a software engineer building an app.