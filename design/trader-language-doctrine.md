---
title: Trader Language Doctrine
status: canonical
type: design-doctrine
created: 2026-05-17
updated: 2026-05-17
tags:
  - TradeForge
  - design
  - trader-language
  - ux
  - cognition
  - terminology
related:
  - UX_DOCTRINE.md
  - GLOSSARY.md
  - DESIGN_ARCHITECTURE.md
  - WORKSPACES.md
authoritative_statement: >
  TradeForge preserves canonical internal semantics while translating
  operator-facing UX into trader-native language that supports cognition.
---

# Trader Language Doctrine

## Purpose

This doctrine defines the boundary between:

- canonical internal semantics
- operator-facing language

TradeForge is a semantic system, so internal terminology must remain stable.
TradeForge is also a trader-facing cognition system, so runtime UX must not force
operators to mentally translate internal ontology before they can think clearly.

## Core Principle

> Canonical semantics govern system truth.
> Operator language supports human cognition.

The same underlying concept may therefore have:

- a canonical internal term used by the domain model, events, ADRs, and replay
- a trader-facing label used in headings, prompts, empty states, and tooltips

This is translation, not semantic drift.

## Boundary Rule

Internal semantics should remain visible when the operator needs:

- authority boundaries
- replay traceability
- provenance inspection
- audit or diagnostic understanding

Internal semantics should not dominate when the operator primarily needs:

- orientation
- opportunity evaluation
- conditional reasoning
- context interpretation
- next-step clarity

## Internal Versus Operator-Facing Language

Examples:

| Canonical Internal Term | Operator-Facing Language Candidate |
| --- | --- |
| `TradeIdea` | trade idea |
| `OpportunityCandidate` | opportunity, setup, developing trade |
| `ScenarioBranch` | bull case, bear case, alternate path, invalidation |
| `Derived State` | current view, current summary, current interpretation |
| `Advisory` | advisory, non-authoritative context |
| `Decision Lifecycle State` | current stage |

The operator-facing term must preserve the underlying meaning while matching the
decision task the user is actually performing.

## Surface Rules

### Headings

Headings should answer the operator's live question, not expose the backing data
shape.

Prefer:

- `Evaluate Opportunity`
- `What requires attention now?`
- `Bull case`

Avoid when used as primary headings:

- `Opportunity Candidates`
- `Scenario Branches`
- `Derived State`

### Prompts

Prompts should guide the operator's reasoning task.

Prefer:

- `What confirms this setup?`
- `What would invalidate the idea?`
- `What information is still missing?`

Avoid:

- prompts that merely restate lifecycle or persistence structure

### Empty States

Empty states should explain:

1. what is absent
2. what that means for the current decision
3. whether work can continue
4. what the operator can do next

Avoid bare system states such as:

- `No source events yet`
- `Fundamentals unavailable`

when the operator-facing task requires interpretation.

### Provenance And Authority

Canonical, derived, inferred, and advisory distinctions remain important.

They should usually appear as:

- badges
- metadata rows
- expandable detail
- replay diagnostics

They should not become the main cognitive structure of a workspace unless the
operator is explicitly inspecting authority or provenance.

## Non-Goals

Trader-language translation does not:

- rename canonical entities
- weaken glossary governance
- hide authority boundaries
- turn advisory interpretation into canonical truth
- replace semantic precision with casual wording

## Design Test

Before shipping operator-facing language, ask:

1. Does this wording preserve canonical meaning?
2. Does it match the operator's actual task?
3. Does it reduce or increase mental translation burden?
4. Would a trader understand the surface before understanding the implementation?

If the answer to the fourth question is no, the language is probably still too
close to internal ontology.

## Related Follow-On Work

- `TF-F029` should use this doctrine to revise Opportunity Workspace copy.
- `TF-F035` should use this doctrine to translate scenario terminology.
- `TF-F039` should use this doctrine when defining missing-information states.
