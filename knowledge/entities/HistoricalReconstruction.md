---
title: HistoricalReconstruction
type: entity
status: canonical
tags: [TradeForge, entity, replay, historical-reconstruction, derived-state]
created: 2026-05-10
updated: 2026-05-10
aliases: [Historical Reconstruction]
related:
  - "[[ReplaySession]]"
  - "[[ReplayTimeline]]"
  - "[[Projection]]"
  - "[[EventLedger]]"
  - "[[ReviewArtifact]]"
---

# HistoricalReconstruction

## Definition

A HistoricalReconstruction is a deterministic, derived replay output that composes historical facts, replay projection state, replay timeline state, and explicitly bounded inferred state.

It is built from ordered [[EventLedger]] history through deterministic replay components.

## Semantic Role

HistoricalReconstruction provides a structured reconstruction of what happened, what derived state can be rebuilt from those facts, and what inferred interpretation is present or intentionally absent at a given replay boundary.

It exists to support replay, review, and historical understanding without creating a second source of truth.

## Authority Boundary

HistoricalReconstruction is derived state, not canonical truth.

It must not:

- append events
- mutate lifecycle state
- persist authoritative reconstruction state
- call live APIs
- generate AI narration as historical truth
- collapse facts, derived state, and inferred state into one undifferentiated output

If reconstruction output conflicts with source events, source events win.

## Composition Relationship

HistoricalReconstruction composes:

- source facts from event history
- derived state from deterministic replay projection
- derived sequence context from [[ReplayTimeline]]
- inferred state as an explicitly bounded layer

For TF-0019, inferred state remains an explicit empty boundary rather than an active interpretation layer.

## Historical Integrity

HistoricalReconstruction should preserve source links for notes, reviews, and other replay-relevant artifacts so those artifacts remain traceable to originating events rather than becoming new facts.

## Related Concepts

- [[ReplaySession]]
- [[ReplayTimeline]]
- [[Projection]]
- [[EventLedger]]
- [[ReviewArtifact]]
