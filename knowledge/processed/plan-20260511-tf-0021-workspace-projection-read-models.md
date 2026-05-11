---
title: Plan - TF-0021 Workspace Projection Read Models
type: processed-planning
status: processed
created: 2026-05-11
source: knowledge/raw/archived/20260511-tf-0021-workspace-projection-read-models-plan.md
issue: TF-0021
milestone: M6
branch: feature/tf-0021-workspace-projection-read-models
related:
  - [[Projection]]
  - [[Persona Workspace]]
  - [[Replay System]]
  - [[Decision Lifecycle State]]
---

# Plan - TF-0021 Workspace Projection Read Models

## Stabilized Planning Summary

TF-0021 implements the executable read-model layer for MVP workspaces. The runtime should translate existing workspace contracts into deterministic, rebuildable projections over ordered event history.

This is implementation of accepted architecture, not a semantic expansion. Workspace read models remain derived state, source-linked, persona/workspace scoped, and discardable.

## Semantic Observations

- Workspace projections are a concrete instance of [[Derived State]].
- Projection output must not be confused with canonical lifecycle truth.
- Persona context shapes projection scope and interpretation metadata without mutating facts.
- Workspace surfaces remain operational cognition environments, not UI tabs or dashboards.

## Architecture Boundaries

The implementation should preserve these boundaries:

- Event Ledger: source of truth.
- Decision Lifecycle Engine: lifecycle authority.
- Workspace projection: derived read model only.
- Persona: interpretation and scoping context only.
- Replay: deterministic reconstruction from event history.

No new canonical event model or lifecycle semantics are required.

## ADR Assessment

No new ADR is required for TF-0021. ADR 0004 and ADR 0012 already authorize derived workspace projections and MVP workspace boundaries. The issue implements those accepted decisions without changing source-of-truth boundaries or replay behavior.

## Runtime Implementation Expectations

Runtime implementation should add:

- immutable workspace projection read models;
- deterministic projection from ordered EventEnvelope history;
- explicit source event references and event types;
- persona/workspace context in each projection;
- compatibility with the existing projection rebuild pipeline;
- tests for deterministic rebuild, persona scoping, lifecycle derivation, and authority boundaries.

## Processing Result

This planning capture does not introduce new ontology, workflow, playbook, or doctrine changes. It is sufficiently captured as processed implementation planning. No canonical KB structures require mutation at this stage.
