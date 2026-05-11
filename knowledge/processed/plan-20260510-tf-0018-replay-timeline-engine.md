---
title: TF-0018 Replay Timeline Engine
type: processed
status: active
created: 2026-05-10
tags:
  - TradeForge
  - runtime
  - TF-0018
  - M5
  - replay
  - timeline
source:
  - knowledge/raw/archived/20260510-tf-0018-replay-timeline-engine-plan.md
related:
  - "[[Runtime KB Development Loop]]"
  - "[[Replay System]]"
  - "[[ReplayTimeline]]"
  - "[[Event Ledger]]"
---

# TF-0018 Replay Timeline Engine

TF-0018 adds a deterministic replay timeline model for lifecycle, execution, review, and relevant system events.

## Stabilized Planning Summary

The timeline engine should be a derived reconstruction layer.

It must consume event history and produce immutable timeline entries.

It must not append events, persist state, call live APIs, depend on UI state, or own lifecycle authority.

## Runtime Boundary

The domain layer owns deterministic timeline construction.

The service layer owns event-store orchestration through the `EventStore` port.

The app/UI layer is not involved in this issue.

## Determinism

Determinism depends on:

- preserving source event sequence
- sorting timeline entries by timestamp and original sequence
- deriving lifecycle stages from canonical lifecycle event mappings
- preserving source references and provenance without reinterpretation

## Historical Integrity

Timeline entries should preserve enough source event context for replay and review surfaces to link back to the facts that created the entry.

This includes source sequence, event type, domain, timestamp, persona/workspace context, entity references, payload, and provenance.

## Deferred Work

The following remain deferred:

- interactive frontend timeline
- persisted timelines
- historical reconstruction composition
- AI replay narration
- API exposure

## Invariant Alignment

The design preserves:

- Replay: timeline derives from event history only
- Event Integrity: source events remain immutable and are not rewritten
- Historical Integrity: entries preserve event references and provenance
- Layer Separation: domain constructs; services orchestrate through ports

## KB Processing Result

This note is a processed planning synthesis.

Stable knowledge promoted from this note:

- replay timeline is a valid stable KB concept distinct from `ReplayTimelineBuilder` and other runtime implementation names.
- replay timeline is derived reconstruction only and does not create lifecycle, event, or persistence authority.
- deterministic ordering by event timestamp plus source event sequence is part of the replay-timeline boundary.
- replay timeline must preserve source event context and provenance for replay and review use.

Ontology impact:

- [[ReplayTimeline]] should be treated as the canonical KB concept.
- runtime names such as `ReplayTimelineBuilder`, `ReplayTimelineEntry`, and `ReplayTimelineService` remain implementation vocabulary.
- no new event taxonomy, lifecycle state, AI authority, or workspace truth concept is introduced.

Workflow and playbook impact:

- TF-0018 continues the M5 replay path after projection and rebuild foundations.
- no playbook change is required.

ADR impact:

- no new ADR is required.
- accepted replay doctrine from ADR 0008 remains sufficient.

Operational synchronization:

- the source raw note has been archived.
- runtime tracking now marks TF-0018 Done, so KB indexes must be synchronized.
