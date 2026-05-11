---
title: TF-0020 Persona Context Model
type: processed
status: active
created: 2026-05-11
tags:
  - TradeForge
  - runtime
  - TF-0020
  - M6
  - persona
  - workspace-projection
source:
  - knowledge/raw/archived/20260511-tf-0020-persona-context-model-plan.md
related:
  - "[[Runtime KB Development Loop]]"
  - "[[Persona]]"
  - "[[Persona Workspace]]"
  - "[[Replay System]]"
---

# TF-0020 Persona Context Model

TF-0020 begins Milestone 6 by translating ADR 0009 persona interpretation doctrine into a pure runtime domain model.

## Stabilized Planning Summary

Persona context should model a persona as a stable interpretation and prioritization context.

It must not model a user account, session, permission set, UI preference, trading mood, lifecycle authority, event writer, or execution authority.

## Runtime Boundary

The domain layer owns immutable persona context contracts.

The model should be framework-free, persistence-free, and service-free. Later M6 issues may consume persona context when building workspace projections, attention queues, or summaries, but TF-0020 must not implement those later behaviors early.

## State Distinction

Persona context is not canonical event truth by itself.

Canonical facts remain in the Event Ledger, including future `persona.*` facts such as activation or version changes. Persona context preserves interpretive inputs so derived systems can apply emphasis and weighting without mutating canonical state.

## Replay Implication

Historical replay must preserve the persona version and workspace/workflow context active at the time. Later changes to a persona must not reinterpret prior workflows implicitly.

## Lifecycle Boundary

Persona context may shape future queue prioritization, approval strictness emphasis, or workspace framing. It must not create, validate, approve, execute, or skip lifecycle transitions.

## Deferred Work

The following remain deferred:

- workspace projection read models
- operational attention queues
- context-aware workspace summaries
- persona persistence
- authentication or session identity
- AI-generated persona interpretation

## Invariant Alignment

The design preserves:

- Persona: personas shape interpretation, not truth
- Workspace: workspace context remains persona-scoped
- Replay: historical context carries persona version explicitly
- Layer Separation: domain contracts do not own services, infrastructure, or app behavior

## KB Processing Result

This note is a processed planning synthesis.

Stable knowledge promoted from this note:

- TF-0020 should implement persona context as immutable domain contracts.
- persona version identity is necessary for replay-safe historical interpretation.
- workspace and workflow references may be associated with persona context without making persona context a state owner.

Ontology impact:

- existing [[Persona]] semantics remain sufficient.
- no new canonical entity is required beyond the established Persona and Persona Workspace concepts.

Workflow and playbook impact:

- TF-0020 starts M6 persona workspace projection groundwork.
- no workflow or playbook update is required.

ADR impact:

- no new ADR is required.
- ADR 0009 remains the controlling implementation authority.

Operational synchronization:

- source raw planning note has been archived.
- runtime issue TF-0020 is now Done.
- runtime roadmap marks TF-0020 Done while M6 remains Planned.
