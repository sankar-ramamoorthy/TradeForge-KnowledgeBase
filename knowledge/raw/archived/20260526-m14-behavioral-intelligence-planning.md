---
title: M14 Behavioral Intelligence Planning
type: raw-capture
status: raw
created: 2026-05-26
tags:
  - TradeForge
  - M14
  - behavioral-intelligence
  - cognitive-auditability
  - runtime-kb-development-loop
---

# M14 Behavioral Intelligence Planning

## Context

M13B completed the managed advisory runtime boundary. The next runtime milestone
is M14: Behavioral Intelligence And Cognitive Auditability.

M14 moves TradeForge from advisory/provider governance into behavioral review:
surfacing recurring process problems, discipline drift, and decision-quality
patterns from existing lifecycle, review, and evidence history.

## Planning Decision

The first M14 implementation slice is TF-C001: Detect recurring sizing
violations.

This is intentionally deterministic and replayable. It uses structured plan and
review event payloads already present in the event ledger rather than adding
new canonical state or AI interpretation.

## Runtime Boundary

Behavioral signals are derived read models:

- canonical inputs: lifecycle, plan, execution, and review events
- derived outputs: deterministic behavioral signals
- advisory outputs: future clustering, narrative interpretation, and AI review

The first slice does not add lifecycle authority, execution authority, hidden
state, or canonical behavioral event types.

## Issue Sequence

Recommended M14 order:

1. TF-C001: Detect recurring sizing violations
2. TF-C002: Detect impulsive execution patterns
3. TF-C003: Implement process deviation overlays
4. TF-C004: Implement behavioral clustering
5. TF-C005: Implement recurring mistake analysis
6. TF-C006: Implement discipline deterioration signals
7. TF-C007: Implement thesis attachment analysis
8. TF-C008: Implement emotional reflection overlays
9. TF-C009: Implement operator behavior timelines
10. TF-C010: Implement decision-quality review metrics

## Implementation Capture

TF-C001 introduced:

- ADR-0044 for behavioral signals as derived read models
- `SizingViolationDetector` in the domain layer
- `BehavioralSignalReadService` in the service layer
- `GET /behavioral/signals` as a read-only derived API
- tests proving recurrence detection, clean-review exclusion, API filtering,
  and no event-ledger writes

## Follow-Up

Process this raw capture through the KB workflow if M14 creates durable semantic
entities such as BehavioralSignal, DisciplineDrift, or CognitiveAuditability.
