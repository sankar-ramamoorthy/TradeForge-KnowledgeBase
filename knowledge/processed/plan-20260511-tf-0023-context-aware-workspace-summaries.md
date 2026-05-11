---
title: Plan - TF-0023 Context-Aware Workspace Summaries
type: processed-planning
status: processed
created: 2026-05-11
source: knowledge/raw/20260511-tf-0023-context-aware-workspace-summaries-plan.md
issue: TF-0023
milestone: M6
branch: feature/tf-0023-context-aware-workspace-summaries
related:
  - [[Persona Workspace]]
  - [[Projection]]
  - [[Derived State]]
  - [[Persona]]
---

# Plan - TF-0023 Context-Aware Workspace Summaries

## Stabilized Planning Summary

TF-0023 implements deterministic, non-AI workspace summaries as derived read models over workspace projections and operational attention queues. Summaries should be concise, source-linked, persona-aware, and replay-compatible.

## Semantic Observations

Workspace summaries are derived state. They may help a workspace surface communicate what matters, but they are not canonical truth, lifecycle authority, execution permission, or AI interpretation.

Persona context may shape emphasis and ordering. This remains interpretive and must not mutate source facts.

## Architecture Boundaries

- Event Ledger remains source of truth.
- Workspace projections remain derived source context.
- Operational attention queues remain derived responsibility context.
- Summaries remain derived and discardable.
- AI-generated summaries remain out of scope.

## ADR Assessment

No new ADR is required. ADR 0004, ADR 0009, and ADR 0012 already cover the projection, persona interpretation, and workspace boundaries needed for deterministic summaries.

## Runtime Implementation Expectations

Runtime implementation should add:

- immutable summary models;
- deterministic summary projection from workspace projections and attention queues;
- explicit input declarations and source references;
- persona-shaped emphasis fields;
- read-service composition through EventStore;
- tests for replay determinism, source traceability, persona influence, and non-authority behavior.

## Processing Result

This planning note stabilizes implementation intent only. It does not introduce new ontology, workflow, playbook, or doctrine changes.
