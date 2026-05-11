---
title: Plan - TF-0022 Operational Attention Queues
type: processed-planning
status: processed
created: 2026-05-11
source: knowledge/raw/archived/20260511-tf-0022-operational-attention-queues-plan.md
issue: TF-0022
milestone: M6
branch: feature/tf-0022-operational-attention-queues
related:
  - [[Persona Workspace]]
  - [[Projection]]
  - [[Decision Lifecycle State]]
  - [[Human Decision Sovereignty]]
---

# Plan - TF-0022 Operational Attention Queues

## Stabilized Planning Summary

TF-0022 implements operational attention as derived queue state. The runtime should produce deterministic queue items that explain why human attention is required while preserving source references and lifecycle authority boundaries.

## Semantic Observations

Operational attention is a workspace projection concern, not canonical truth. Attention items are derived signals of responsibility or review need; they do not authorize execution, approval, or lifecycle mutation.

Persona context may influence ordering and emphasis, but that influence remains interpretive. The implementation must keep queue priority deterministic and replay-compatible.

## Architecture Boundaries

- Event Ledger remains source of truth.
- Decision Lifecycle Engine remains lifecycle authority.
- Workspace projections provide source-derived context.
- Attention queues are discardable derived state.
- AI prioritization remains out of scope.

## ADR Assessment

No new ADR is required. ADR 0013 already defines operational attention as derived queue state and rejects queue authority over execution or lifecycle changes.

## Runtime Implementation Expectations

Runtime implementation should add:

- immutable operational attention queue models;
- stable attention categories, reasons, and priority levels;
- deterministic queue projection from workspace projections and event history;
- source event references for every item;
- explicit authority boundaries on queue output;
- read-service composition through the existing EventStore and workspace projection layer;
- tests for deterministic ordering, persona-aware weighting, replay compatibility, and non-authority behavior.

## Processing Result

This planning note stabilizes implementation intent only. It does not introduce new ontology or canonical doctrine. No KB canonical structures require mutation before runtime implementation.
