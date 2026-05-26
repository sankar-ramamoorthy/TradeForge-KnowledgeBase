---
title: BehavioralSignal
type: entity
status: canonical
tags: [TradeForge, entity, behavioral-intelligence, review, M14]
created: 2026-05-26
updated: 2026-05-26
milestone: M14
runtime-issues: [TF-C001]
source_history:
  - knowledge/processed/20260526-m14-behavioral-intelligence-planning-synthesis.md
  - ../../../../TradeForge/DOCS/adr/0044-behavioral-signal-derived-read-model.md
---

# BehavioralSignal

## Definition

A BehavioralSignal is a deterministic derived read model that identifies a
review-relevant behavior or process pattern from Event Ledger history.

It is used to make discipline concerns, process deviations, and recurring review
patterns visible without changing lifecycle state or creating canonical
behavioral facts.

## Authority

A BehavioralSignal is derived state.

It is NOT:

- canonical truth
- a lifecycle event
- lifecycle authority
- execution authority
- an approval gate
- an AI-authored conclusion
- a persistent hidden score

Behavioral signals must preserve explicit source event references so operators
can inspect the canonical facts that produced the signal.

## Required Context

A BehavioralSignal should preserve:

- signal identity
- signal type
- severity
- persona context
- workspace context, when available
- decision reference
- summary and rationale
- recurrence count
- detected timestamp
- source event references
- explicit derived/non-canonical authority metadata

## Replay Meaning

Replay may reconstruct BehavioralSignals from historical event history and show
them as review context.

Replay must distinguish:

- canonical source events
- deterministic derived behavioral signal
- later operator review or interpretation

## Runtime Stabilization

TF-C001 introduced the first BehavioralSignal implementation for recurring
sizing violations. ADR-0044 establishes behavioral signals as deterministic
derived read models rather than new event-ledger facts.

Future M14 work may add additional signal types, overlays, timelines, and
analysis layers, but those must preserve the same authority boundary.

## Related Concepts

- [[ReviewArtifact]]
- [[ReplaySession]]
- [[Projection]]
- [[Derived State]]
- [[Decision Lifecycle State]]
- [[Behavioral Intelligence]]

