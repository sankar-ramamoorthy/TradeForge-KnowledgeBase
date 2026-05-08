---
title: Codex Operating Boundaries
type: note
status: processed
tags: [TradeForge, Codex, skills, prompts, operating-boundaries]
created: 2026-05-08
updated: 2026-05-08
---

# Codex Operating Boundaries

## Purpose

This note preserves operating guidance for where Codex should work and how to distinguish skills from prompts in the TradeForge knowledge system.

---

## Repository Boundary Rule

Use the knowledge base when modifying semantic and cognitive structure:

- semantic doctrine
- prompts
- ontology
- skills
- workflows
- cognition structure
- durable architectural memory

Use the runtime repository when modifying implementation structure:

- runtime ADRs
- services
- domain models
- runtime documentation
- source code
- implementation tests

The practical split is:

- `knowledge-base/TradeForge/` preserves why concepts exist and what they mean.
- `TradeForge/` implements how the system behaves and when work is sequenced.

---

## Skills Versus Prompts

Skills and prompts should remain separate because they operate at different layers.

Skills define how Codex should think:

- behavioral modifiers
- reasoning constraints
- cognitive overlays
- domain operating rules
- architectural discipline

Examples:

- decision lifecycle reasoning
- event-sourcing discipline
- execution discipline
- market interpretation
- scenario ranking
- workspace querying

Skills belong in:

```text
skills/
```

Prompts define what Codex should do:

- reusable workflows
- operational procedures
- structured task entrypoints

Examples:

- generate ADRs
- build a vertical slice
- replay analysis

Prompts belong in:

```text
prompts/
```

---

## Design Rule

Do not merge skills and prompts.

The durable distinction is:

```text
skills  = reusable thinking constraints
prompts = reusable operational workflows
```

This separation protects terminology discipline and keeps behavioral guidance distinct from task execution.

