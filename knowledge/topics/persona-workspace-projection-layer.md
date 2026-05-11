---
title: Persona Workspace Projection Layer
type: topic
status: draft
tags:
  - TradeForge
  - topic
  - persona-workspace
  - projection
  - derived-state
  - M6
created: 2026-05-11
updated: 2026-05-11
source:
  - knowledge/processed/plan-20260511-tf-0021-workspace-projection-read-models.md
  - knowledge/processed/implemented-20260511-tf-0021-workspace-projection-read-models.md
  - knowledge/processed/plan-20260511-tf-0022-operational-attention-queues.md
  - knowledge/processed/implemented-20260511-tf-0022-operational-attention-queues.md
  - knowledge/processed/plan-20260511-tf-0023-context-aware-workspace-summaries.md
  - knowledge/processed/implemented-20260511-tf-0023-context-aware-workspace-summaries.md
related:
  - "[[Persona]]"
  - "[[Workspace]]"
  - "[[Projection]]"
  - "[[DecisionLifecycleState]]"
  - "[[ReplaySession]]"
  - "[[Development Replayability]]"
  - "[[MILESTONE_ROADMAP]]"
  - "[[ISSUE_REGISTER]]"
---

# Persona Workspace Projection Layer

## Definition

The Persona Workspace Projection Layer is the M6 runtime capability that turns ordered Event Ledger history into persona-scoped, replay-compatible workspace read models.

It is composed of:

- workspace projection read models
- operational attention queues
- context-aware workspace summaries

These artifacts are derived state. They help Persona Workspaces communicate context, responsibility, and emphasis without becoming canonical truth, lifecycle authority, execution permission, or AI interpretation.

## Stabilized M6 Result

TF-0021 through TF-0023 completed the first runtime layer where Persona Workspace cognition becomes executable as deterministic projections.

Semantic result:

- TF-0021 established workspace projections as immutable, persona/workspace-scoped read models over ordered EventEnvelope history.
- TF-0022 established operational attention queues as deterministic responsibility surfaces explaining why human attention is required.
- TF-0023 established context-aware workspace summaries as deterministic, non-AI summaries over workspace projections and attention queues.

The three issues form a derived-state chain:

```text
Event Ledger history
    -> workspace projections
    -> operational attention queues
    -> workspace summaries
    -> workspace surfaces
```

Each step remains rebuildable and non-authoritative.

## Authority Boundary

This layer does not:

- append Event Ledger events
- approve or reject lifecycle transitions
- execute trades
- define new event taxonomy
- introduce projection persistence
- call live APIs for replay
- use AI-generated summaries or priorities
- replace source event history

If derived workspace output conflicts with Event Ledger history, Event Ledger history wins.

## Persona Relationship

Persona context shapes interpretation, emphasis, and deterministic ordering. It does not mutate facts.

Persona influence must remain replay-safe:

- persona version identity should be explicit
- workspace and workflow context should be scoped
- the same event history plus the same persona context should produce the same derived output

## Replay Relationship

The layer reinforces replayability because workspace context, attention, and summaries can be rebuilt from historical inputs.

Replay should treat this layer as reconstruction support, not historical truth by itself. Replay truth still comes from events, deterministic rules, and historical snapshots where available.

## Doctrine Alignment

The M6 processing pass found no contradiction with canonical doctrine.

Aligned doctrine:

- [[INVARIANTS]]: derived state remains distinguishable from canonical state.
- [[ARCHITECTURE]]: workspaces remain projections over event-backed context.
- [[UX_DOCTRINE]]: context before action is implemented through attention and summary surfaces.
- [[EVENT_TAXONOMY]]: no new event category or event authority is introduced.
- [[GLOSSARY]]: Persona, Workspace, Projection, Operational Attention Queue, Workspace Summary, and Decision Lifecycle State boundaries remain explicit.

## ADR Alignment

No new ADR is required for this processing pass.

Existing ADRs cover the accepted architecture:

- ADR 0004: workspace projections are derived read models.
- ADR 0007: workspace surfaces must avoid dashboard-first UX.
- ADR 0008: replay depends on deterministic reconstruction.
- ADR 0009: persona interpretation is bounded and non-authoritative.
- ADR 0012: MVP workspaces are operational cognition environments.
- ADR 0013: operational attention is derived queue state.
- ADR 0014: replay-centric UX consumes deterministic projections.

## Operational Synchronization

Runtime alignment as of 2026-05-11:

- TF-0021: Done
- TF-0022: Done
- TF-0023: Done
- M6: Done
- M7: next planned milestone

Operational drift found during processing:

- KB milestone and issue indexes still listed TF-0021 through TF-0023 as planned before this pass.
- Processed notes pointed to pre-archive raw note paths.

This processing pass updates those KB records and preserves the archived raw source paths in processed front matter.

## Canonicalization Status

This topic is draft-level synthesis, not new doctrine.

The stable canonical concepts are:

- [[Persona]]
- [[Workspace]]
- [[Projection]]
- [[OperationalAttentionQueue|Operational Attention Queue]]
- [[WorkspaceSummary|Workspace Summary]]
- [[DecisionLifecycleState]]
- [[ReplaySession]]

Potential future doctrine promotion should wait until M7/M8 validates the layer through API and workspace UI usage.

## Open Questions

- Should Operational Attention Queue require additional workflow-specific queue categories after M8 workspace usage validates the interaction model?
- Should Workspace Summary become a documented workspace surface contract after API and frontend usage expose its stable shape?
- Should the M8 first operational MVP vertical slice add a dedicated replay check for summary and attention reconstruction?
