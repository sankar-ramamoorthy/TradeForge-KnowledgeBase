# TradeForge — Execution Contract

## Purpose

This document defines how Codex moves from:

> knowledge-base reasoning → runtime code implementation

It enforces controlled translation of architecture into code.

---

# Core Principle

Codex does NOT write code directly from memory.

Codex must:

1. Interpret knowledge-base rules
2. Produce an implementation plan
3. Validate against skills + invariants
4. Only then generate code

---

# Execution Pipeline

All implementation must follow:

```text
1. Context Load
2. Skill Activation
3. Architectural Interpretation
4. Design Plan Generation
5. Validation Against Invariants
6. Code Output
```

---

# Step 1 — Context Load

Codex must load:

- ARCHITECTURE.md
- INVARIANTS.md
- GLOSSARY.md
- EVENT_TAXONOMY.md
- relevant skills

No step may be skipped.

---

# Step 2 — Skill Activation

Relevant skills MUST be selected:

Examples:
- event-sourcing.md
- decision-lifecycle.md
- workspace-querying.md

Skills act as **execution constraints**, not suggestions.

---

# Step 3 — Architectural Interpretation

Codex must translate requirements into:

- domain model changes
- event model changes
- workflow changes
- service boundaries

NOT code yet.

---

# Step 4 — Design Plan Generation

Codex must produce:

- module structure
- class responsibilities
- event flows
- data flow diagrams (ASCII allowed)
- lifecycle transitions

This is a required checkpoint.

---

# Step 5 — Validation Against Invariants

Before any code is produced:

Codex must verify:

- event sourcing integrity preserved
- lifecycle rules respected
- workspace model not violated
- AI boundaries not broken
- glossary consistency maintained

If violated → STOP.

---

# Step 6 — Code Output

Only after validation:

Codex may generate code for runtime repo.

Code must:
- map directly to design plan
- avoid hidden assumptions
- respect module boundaries

---

# Hard Rule

Codex MUST NOT:

- skip design phase
- generate code directly from request
- bypass invariant validation
- collapse multiple steps into one

---

# Anti-Pattern

Bad:

> “Create TradePlan service”

Correct:

> “Define lifecycle impact → validate against decision model → design service → then implement”

---

# Final Principle

> Architecture is designed in the knowledge-base. Code is executed in the runtime repo.