---
title: TF-0019 Historical Reconstruction Pipeline
type: processed
status: active
created: 2026-05-10
tags:
  - TradeForge
  - runtime
  - TF-0019
  - M5
  - replay
  - historical-reconstruction
source:
  - knowledge/raw/20260510-tf-0019-historical-reconstruction-pipeline-plan.md
related:
  - "[[Runtime KB Development Loop]]"
  - "[[Replay System]]"
  - "[[Replay Timeline]]"
  - "[[Event Ledger]]"
---

# TF-0019 Historical Reconstruction Pipeline

TF-0019 composes the Milestone 5 replay foundations into a historical reconstruction service.

## Stabilized Planning Summary

Historical reconstruction should remain a derived service output.

It composes event-backed facts, replay projection, and replay timeline output.

It must not create canonical truth, mutate lifecycle state, persist reconstruction state, call live APIs, or generate AI narration.

## Runtime Boundary

The services layer owns reconstruction composition.

The domain layer continues to own deterministic replay projection and timeline construction.

The infrastructure layer remains behind the `EventStore` port.

## State Distinction

The reconstruction output should keep separate:

- facts from source events
- derived state from deterministic replay components
- inferred state, explicitly empty for TF-0019

This preserves the TradeForge distinction between canonical facts, derived state, and inferred interpretation.

## Historical Integrity

Notes and review artifacts should remain linked to their source events.

They should not become new facts separate from the events that carried them.

## Deferred Work

The following remain deferred:

- inferred reconstruction layers
- AI replay narration
- persistent reconstruction storage
- replay API endpoints
- frontend replay workspace

## Invariant Alignment

The design preserves:

- Replay: reconstruction derives from event history only
- Historical Integrity: source facts and provenance remain visible
- Layer Separation: services compose; domain reconstructs; infrastructure stays behind ports
- Derived State Distinction: reconstruction output is non-authoritative
