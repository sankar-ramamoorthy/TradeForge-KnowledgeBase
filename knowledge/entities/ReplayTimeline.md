---
title: ReplayTimeline
type: entity
status: canonical
tags: [TradeForge, entity, replay, timeline, derived-state]
created: 2026-05-10
updated: 2026-05-10
aliases: [Replay Timeline]
related:
  - "[[ReplaySession]]"
  - "[[EventLedger]]"
  - "[[LifecycleEvent]]"
  - "[[Projection]]"
  - "[[ReviewArtifact]]"
---

# ReplayTimeline

## Definition

A ReplayTimeline is a deterministic, derived reconstruction of replay-relevant events arranged into immutable timeline entries.

It is built from ordered [[EventLedger]] history and deterministic mapping rules.

## Semantic Role

ReplayTimeline makes historical cognition inspectable across lifecycle, execution, review, and relevant system events without turning UI state or current projections into truth.

It helps replay and review surfaces follow what happened, in what order, and with what source context.

## Authority Boundary

ReplayTimeline is derived state, not canonical truth.

It must not:

- append events
- mutate lifecycle state
- validate requested lifecycle transitions
- depend on live APIs
- depend on current AI output
- depend on mutable UI state
- become persisted authority over EventLedger history

If timeline output conflicts with source events, source events win.

## Construction Relationship

ReplayTimeline depends on:

- ordered event history
- deterministic entry construction
- stable ordering by event timestamp and source event sequence
- preserved source references and provenance

Timeline construction remains descriptive, not authoritative.

## Replay And Review Relevance

ReplayTimeline supports replay and review by preserving source event context needed to inspect:

- lifecycle progression
- execution feedback
- review progression
- relevant system events

Each timeline entry should preserve enough historical context to link interpretation back to the originating facts.

## Related Concepts

- [[ReplaySession]]
- [[EventLedger]]
- [[LifecycleEvent]]
- [[Projection]]
- [[ReviewArtifact]]
