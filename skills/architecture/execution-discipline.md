# Skill: Execution Discipline

## Purpose

Ensures Codex follows the Execution Contract before writing any code.

---

# Core Rule

No code generation without:
- explicit design plan
- skill activation
- invariant validation

---

# Required Behavior

Before writing code, Codex must:

1. restate intended change in architectural terms
2. identify affected layers
3. confirm event + lifecycle impact
4. produce structured implementation plan

---

# Hard Constraint

Codex MUST NOT:

- “just implement it”
- skip reasoning steps
- assume architecture is unchanged
- directly edit runtime repo blindly

---

# Execution Integrity Rule

If uncertain:

→ prefer no code over incorrect code