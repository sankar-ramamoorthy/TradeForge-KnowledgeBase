---
title: TF-0012 Lifecycle Transition Validator
type: processed-synthesis
status: processed
tags: [TradeForge, runtime, TF-0012, decision-lifecycle, validation, replayability]
created: 2026-05-09
updated: 2026-05-09
related:
  - "[[DecisionLifecycleState]]"
  - "[[LifecycleEvent]]"
  - "[[EventLedger]]"
  - "[[ReplaySession]]"
  - "[[TF-0011 Lifecycle State Model]]"
source_history:
  - knowledge/raw/plan-20260509-tf-0012-lifecycle-transition-validator.md processed and removed after synthesis
---

# TF-0012 Lifecycle Transition Validator

## Status

Processed planning synthesis for TF-0012.

## Issue Context

- Issue: TF-0012
- Milestone: M3
- Runtime branch: `feature/tf-0012-lifecycle-transition-validator`
- Affected layer: domain
- Linked ADRs: ADR 0002 and ADR 0003
- Impacted invariants: Decision Lifecycle, Event Integrity, Replay, Layer Separation

## Planning Summary

TF-0012 should implement deterministic lifecycle transition validation as a pure domain concern. The validator builds on [[DecisionLifecycleState]] from TF-0011.

Allowed lifecycle progression is exactly:

```text
None -> Idea -> Thesis -> Plan -> Approval -> Execution -> Position -> Review
```

No stage may be skipped, repeated, regressed, merged, or silently inferred.

## Architecture Boundary

This issue must not introduce service orchestration, event append behavior, persistence, UI behavior, broker behavior, AI behavior, or new event envelope fields.

The validator evaluates whether a requested next stage is acceptable for a supplied current lifecycle state. It does not create events and does not mutate lifecycle history.

## Replay Implications

Validation is deterministic and replay-compatible because it depends only on:

- canonical lifecycle stage order
- supplied current derived lifecycle state
- requested next stage

It must not depend on live APIs, projections, UI state, broker state, or AI output.

## ADR Decision

No new ADR is required. ADR 0002 already defines lifecycle order and rejects shortcuts such as `Idea -> Position`, `Scenario -> Execution`, and `Plan -> Position`. ADR 0003 already preserves event-domain boundaries.

## Validation Plan

Runtime validation should include:

- all valid adjacent transitions accepted
- initial transition requires `Idea`
- invalid shortcuts rejected
- repeated stages rejected
- regressions rejected
- transitions after `Review` rejected
- validator value objects are immutable
- lifecycle module remains free of infrastructure, services, and app dependencies

Required commands:

- `uv run pytest`
- `uv run ruff check .`
- `uv run mypy src tests`

## Follow-On Work

TF-0013 should use this domain validator from a service orchestration boundary before appending lifecycle events through the event store port.
