---
title: TF-0006 Through TF-0008 Runtime Alignment
type: processed-synthesis
status: processed
tags: [TradeForge, runtime, TF-0006, TF-0007, TF-0008, event-model]
created: 2026-05-09
updated: 2026-05-09
source_history:
  - knowledge/raw/plan-20260509-tf-0006-through-tf-0008.md processed and removed after synthesis
---

# TF-0006 Through TF-0008 Runtime Alignment

## Status

Processed runtime alignment note. This synthesis records what the raw planning
note contributed and how the completed runtime implementation aligns with
TradeForge doctrine.

This note is not canonical semantic architecture. Canonical event meaning remains
defined by [[EVENT_TAXONOMY]], [[INVARIANTS]], ontology pages, and accepted ADRs.

## Summary

The raw note planned three runtime issues:

- TF-0006: add lint, type, and development command conventions.
- TF-0007: document README developer setup.
- TF-0008: define the domain-only event envelope and canonical event domains.

Runtime issue status now shows TF-0006, TF-0007, and TF-0008 as done in:

```text
C:\Users\bosto\dockerstuff\TradeForge\DOCS\ISSUE_REGISTER.md
```

## TF-0006 Outcome

TF-0006 established local command conventions for implementation quality:

- `uv run pytest`
- `uv run ruff check .`
- `uv run mypy src tests`

The semantic boundary is unchanged:

- `uv`, pytest, ruff, and mypy are execution-environment tools.
- Tooling does not define [[Event Ledger]] semantics.
- Tooling does not define [[Decision Lifecycle Engine]] authority.
- Tooling does not define [[Persona]], [[Workspace]], replay, or AI governance.

## TF-0007 Outcome

TF-0007 added runtime README setup guidance for local development:

- dependency sync
- test execution
- lint execution
- type checking
- Docker Compose config/build/run commands

The README explicitly points developers back to:

- runtime `AGENTS.md`
- runtime `DOCS/ISSUE_REGISTER.md`
- runtime `DOCS/adr/0011-runtime-development-environment.md`

This preserves the architectural split between environment setup and domain
meaning.

## TF-0008 Outcome

TF-0008 added a domain-only event model under:

```text
C:\Users\bosto\dockerstuff\TradeForge\src\domain\events\
```

The runtime model defines canonical event domain identifiers:

- `persona`
- `workspace`
- `market`
- `scenario`
- `decision`
- `execution`
- `review`
- `system`

The runtime event envelope carries:

- event type
- timestamp
- persona id
- workspace id
- entity references
- payload
- provenance

The event envelope is framework-free and domain-scoped. It does not introduce:

- event store persistence
- infrastructure adapters
- broker integration
- API entrypoints
- lifecycle orchestration

## Semantic Alignment

TF-0008 aligns with [[EVENT_TAXONOMY]] by modeling events as immutable,
contextually grounded facts grouped by canonical domain.

The implementation supports, but does not replace, these knowledge-base truths:

- events are facts, not interpretations
- historical events must be immutable
- replay depends on event-backed history
- lifecycle authority remains with the [[Decision Lifecycle Engine]]
- scenario and AI outputs remain advisory unless converted through proper
  workflow-controlled events

## Open Follow-On Work

TF-0008 establishes the event envelope and domain taxonomy only. It does not
complete M2.

Remaining M2 runtime work:

- TF-0009: define the append-only event store interface
- TF-0010: implement the in-memory event store adapter

These follow-on issues must preserve append-only Event Ledger semantics and must
not normalize historical mutation, deletion, or projection-authored truth.

## Contradictions

No contradiction was found between the raw note, runtime implementation status,
and the knowledge-base semantic doctrine.

## KB Boundary

This processed note records implementation alignment. It does not promote raw
planning language into ontology, alter canonical event definitions, or redefine
TradeForge doctrine.
