---
id: skill-execution-discipline
title: Execution Discipline
category: architecture
applies_to:
  - codex
  - claude
version: 1.0
description: Ensures agents follow the Execution Contract — explicit design plan, skill activation, and invariant validation — before writing any code.
---

# Skill: Execution Discipline

## Purpose

Ensures agents follow the Execution Contract before writing any code.

---

# Core Rule

No code generation without:
- explicit design plan
- skill activation
- invariant validation

---

# Required Behavior

Before writing code, Agent must:

1. restate intended change in architectural terms
2. identify affected layers
3. confirm event + lifecycle impact
4. produce structured implementation plan

---

# Hard Constraint

Agent MUST NOT:

- “just implement it”
- skip reasoning steps
- assume architecture is unchanged
- directly edit runtime repo blindly

---

# Execution Integrity Rule

If uncertain:

→ prefer no code over incorrect code