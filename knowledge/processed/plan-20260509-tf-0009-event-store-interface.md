---
title: TF-0009 Event Store Interface
type: processed-synthesis
status: processed
tags: [TradeForge, runtime, TF-0009, event-ledger, replayability]
created: 2026-05-09
updated: 2026-05-09
related:
  - "[[EventLedger]]"
  - "[[EventStorePort]]"
  - "[[ReplaySession]]"
  - "[[LifecycleEvent]]"
  - "[[EVENT_TAXONOMY]]"
  - "[[INVARIANTS]]"
source_history:
  - knowledge/raw/20260509-tf-0009-event-store-interface.md processed and removed after synthesis
---

# TF-0009 Event Store Interface

## Status

Processed runtime implementation note for TF-0009. This synthesis records the
event store interface boundary added to the runtime and its alignment with
TradeForge event sourcing doctrine.

This note is not canonical semantic architecture. Canonical event and replay
meaning remains defined by [[EVENT_TAXONOMY]], [[INVARIANTS]], and accepted
runtime ADRs.

## Summary

TF-0009 added a domain event store port under the runtime event package. The
port defines the minimum Event Ledger contract needed by later services and
adapters:

- append one immutable event
- read event history in deterministic replay order

Runtime issue status now shows TF-0009 as done in:

```text
C:\Users\bosto\dockerstuff\TradeForge\DOCS\ISSUE_REGISTER.md
```

## Semantic Alignment

The interface supports the Event Ledger as canonical truth without introducing
infrastructure persistence behavior into the domain layer.

The boundary is intentionally narrow:

- no historical update
- no historical delete
- no overwrite or truncation operation
- no projection-authored state
- no database-backed strategy

Historical correction remains modeled as future events, not mutation of prior
events.

## ADR Decision

No new ADR was required. Existing decisions already govern this work:

- ADR 0001: Event Sourcing Core Model
- ADR 0003: Canonical Event Taxonomy
- ADR 0008: Replay System Design

TF-0009 implements an interface implied by those ADRs rather than introducing a
new source-of-truth boundary or persistence strategy.

## Replay Implications

The port gives replay systems and future orchestration services a deterministic
read contract. Replay must still depend only on event history and deterministic
rules, not live APIs, projections, current AI output, or hidden service state.

No lifecycle semantics changed. No new event types were introduced.

## Validation

Runtime validation completed:

- `uv run pytest`
- `uv run ruff check .`
- `uv run mypy src tests`

## Follow-On Work

TF-0010 should implement the in-memory event store adapter against this port.
That work should test concrete append-only behavior and deterministic read
ordering without expanding into database persistence or distributed event
streaming.

## Ontology Implications

TF-0009 introduces a runtime interface concept that should remain distinct from
the canonical [[EventLedger]] concept.

Stable ontology distinctions:

- [[EventLedger]] is canonical truth.
- [[EventStorePort]] is a runtime port that exposes append and deterministic
  history-read behavior.
- an event-store adapter is infrastructure that implements the port.
- a projection is derived state and must not become the event store.
- a replay projector consumes ordered event history but does not own it.

The durable semantic boundary is:

```text
EventLedger     = canonical historical truth
EventStorePort  = runtime interface to append/read event history
Adapter         = concrete infrastructure implementation
Projection      = derived read model
ReplaySession   = deterministic reconstruction over history
```

This distinction prevents ontology drift where a storage mechanism, adapter, or
projection becomes confused with canonical event truth.

## Workflow Implications

TF-0009 strengthens the implementation workflow for future event-backed runtime
issues.

Future runtime work should follow this sequence:

```text
validated domain action
    -> lifecycle or domain rule evaluation
    -> append immutable event through EventStorePort
    -> read ordered event history for replay/projection
```

Workflow implications:

- TF-0010 should implement the in-memory adapter without expanding the domain
  contract.
- TF-0013 should use the port only after lifecycle validation has accepted a
  transition.
- TF-0014 replay projector work should consume deterministic event history, not
  mutable projections or live APIs.
- corrections should be represented as new events, not interface-level mutation.

## Playbook Implications

The runtime development playbook should continue treating event-store work as a
source-of-truth boundary checkpoint.

Playbook-level guidance implied by TF-0009:

- every event-store-related issue should verify that append-only behavior is
  preserved.
- event-store adapters must not add historical update, delete, overwrite, or
  truncation semantics.
- domain ports should stay narrower than infrastructure capabilities.
- tests for adapters should demonstrate deterministic read ordering.
- implementation notes should explicitly state whether lifecycle semantics,
  event types, or replay behavior changed.

No immediate playbook rewrite is required beyond preserving this guidance in
processed knowledge and cross-links.

## Doctrine Integration Opportunities

TF-0009 reinforces existing doctrine rather than changing it.

Doctrine alignment:

- [[INVARIANTS]]: Event Ledger canonical truth, events are immutable,
  replayability is foundational, and lifecycle authority remains separate.
- [[EVENT_TAXONOMY]]: events remain fact-first, append-only, and replayable.
- [[ARCHITECTURE]]: Event Ledger owns canonical truth while projections and
  read models remain derived.
- ADR 0001: the Event Ledger is append-only canonical truth.
- ADR 0003: event domains remain bounded and semantically narrow.
- ADR 0008: replay depends on ordered event history and deterministic rules.

No doctrine change is required.

## Cross-Linking Opportunities

Stable cross-links created or recommended:

- [[EventLedger]]
- [[EventStorePort]]
- [[ReplaySession]]
- [[LifecycleEvent]]
- [[TF-0006 Through TF-0008 Runtime Alignment]]
- [[Development Replayability]]

Future M2 processing should link TF-0010 back to [[EventStorePort]] and
[[EventLedger]] so adapter behavior remains separated from canonical truth.

## Contradictions

No contradiction was found between runtime implementation, issue scope, and the
knowledge-base semantic doctrine.
