---
title: TF-0013 Lifecycle Orchestration Service
type: processed-synthesis
status: processed
tags: [TradeForge, runtime, TF-0013, decision-lifecycle, services, event-sourcing]
created: 2026-05-09
updated: 2026-05-09
related:
  - "[[DecisionLifecycleState]]"
  - "[[LifecycleTransitionValidator]]"
  - "[[LifecycleEvent]]"
  - "[[EventStorePort]]"
  - "[[EventLedger]]"
  - "[[ReplaySession]]"
source_history:
  - knowledge/raw/plan-20260509-tf-0013-lifecycle-orchestration-service.md processed and removed after synthesis
---

# TF-0013 Lifecycle Orchestration Service

## Status

Processed planning synthesis for TF-0013.

## Issue Context

- Issue: TF-0013
- Milestone: M3
- Runtime branch: `feature/tf-0013-lifecycle-orchestration-service`
- Affected layer: services
- Linked ADRs: ADR 0001, ADR 0002, ADR 0003
- Impacted invariants: Event Sourcing, Decision Lifecycle, Event Integrity, Layer Separation

## Planning Summary

TF-0013 should implement a services-layer lifecycle orchestration service. The service coordinates existing domain and event-store components:

```text
EventStore.read_events()
    -> derive_lifecycle_state(...)
    -> validate_lifecycle_transition(...)
    -> append EventEnvelope only when valid
```

The service must not redefine lifecycle transition rules. TF-0012 remains the domain validation authority.

## Architecture Boundary

This issue introduces service orchestration only. It must not introduce UI workflows, live broker execution, durable persistence, API/app entrypoints, new event envelope fields, or new lifecycle domain rules.

The service may depend on the domain event store port and domain lifecycle objects. It must not import infrastructure adapters or manage persistence details.

## Event Emission

Accepted transitions should append canonical lifecycle event types:

| Requested stage | Event type |
|---|---|
| Idea | `decision.trade_idea_created` |
| Thesis | `decision.thesis_created` |
| Plan | `decision.plan_created` |
| Approval | `decision.plan_approved` |
| Execution | `execution.order_submitted` |
| Position | `execution.position_opened` |
| Review | `review.review_completed` |

Invalid transitions append nothing.

## Replay Implications

The orchestrator remains replay-compatible because it derives current lifecycle state from ordered event history before validation. Accepted transitions become immutable ledger facts through the EventStore port.

## ADR Decision

No new ADR is required. ADR 0001 governs append-only event truth, ADR 0002 governs lifecycle validation and authority, and ADR 0003 governs event taxonomy.

## Validation Plan

Runtime validation should include:

- valid transitions append canonical lifecycle events.
- invalid transitions do not append events.
- service uses domain validation rather than duplicating rules.
- service does not import infrastructure, app, UI, broker, or persistence adapters.
- emitted events preserve request context, references, payload, and provenance.

Required commands:

- `uv run pytest`
- `uv run ruff check .`
- `uv run mypy src tests`
