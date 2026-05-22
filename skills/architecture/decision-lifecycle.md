---
id: skill-decision-lifecycle
title: Decision Lifecycle
category: architecture
applies_to:
  - codex
  - claude
version: 1.0
description: Governs how decisions move through TradeForge lifecycle states and enforces strict stage progression.
---

# Skill: Decision Lifecycle

## Purpose

Defines how decisions move through TradeForge lifecycle states.

---

# Core Principle

Decisions are:
> stateful workflows governed by explicit transitions

---

# Lifecycle Rule

All decisions must follow:

```text
Idea → Thesis → Plan → Approval → Execution → Position → Review
```

No shortcuts allowed.

---

# Transition Rule

Transitions MUST:

- be explicit
- be event-backed
- be validated by lifecycle engine

---

# Hard Constraint

Agent MUST NOT:

- skip lifecycle stages
- collapse multiple stages into one
- directly create positions from ideas

---

# State Ownership

Only the Decision Lifecycle Engine may:

- advance lifecycle state
- validate transitions
- enforce workflow rules

---

# Invalid Pattern

Bad:
> “Convert idea into position”

Correct:
> “Idea must progress through thesis and plan stages before execution”

---

# Review Requirement

Every completed lifecycle must produce:

- review event
- outcome evaluation
- rule feedback loop

---

# Final Principle

No decision exists outside the lifecycle.