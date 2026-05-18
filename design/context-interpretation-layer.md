---
title: Context Interpretation Layer
status: canonical
type: design-architecture
created: 2026-05-17
updated: 2026-05-17
tags:
  - TradeForge
  - design
  - context
  - interpretation
  - advisory
related:
  - ADR-0039
  - UX_DOCTRINE.md
  - missing-information-doctrine.md
  - trader-language-doctrine.md
authoritative_statement: >
  TradeForge separates advisory context acquisition from advisory context
  interpretation so provider payloads become operator cognition without becoming
  canonical truth.
---

# Context Interpretation Layer

## Purpose

Define how normalized advisory context becomes operator-readable cognition.

## Inputs

- normalized price artifacts
- normalized fundamentals artifacts
- future normalized context families
- provider provenance
- retrieval state and failure context

## Outputs

- concise operator-readable interpretation
- relevance to the current decision task
- uncertainty and contradiction notes
- missing-evidence notes
- provenance references back to source artifacts

## Authority Boundary

Interpretations are:

- derived
- advisory
- replay-preservable when based on historical inputs

Interpretations are not:

- events
- lifecycle authority
- execution authority
- hidden replacements for raw artifacts

## Design Pattern

```text
provider artifacts
    -> normalized context
    -> context interpretation
    -> workspace cognition surface
```

## Deterministic Versus AI Interpretation

`M10E` establishes the layer itself.

- deterministic interpretation may be used where explicit rules exist
- future AI interpretation may contribute advisory artifacts later
- both must remain distinguishable from canonical truth and from each other

## Workspace Implications

Workspaces should consume interpretation when the operator needs meaning, not
force the operator to derive meaning from raw payloads first.

Examples:

- range-bound versus trending price structure
- valuation context completeness
- contradiction between technical strength and weak fundamentals
- missing confirmation before thesis development

