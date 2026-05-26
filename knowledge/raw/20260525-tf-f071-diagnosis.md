---
title: TF-F071 Diagnosis - Advisory Thesis Review Event Store Loading
type: raw-diagnosis
status: raw
created: 2026-05-25
tags: [TradeForge, TF-F071, feedback, advisory, event-store, diagnosis]
source:
  - knowledge/raw/Generate-advisory-resulting-in error.md
issues:
  - TF-F071
---

# Observation

After TF-F070, the frontend can reach `POST /advisory/thesis-review`, but the
backend returns `500 Internal Server Error` before invoking LiteLLM.

The source log shows:

```text
AttributeError: 'PostgresEventStore' object has no attribute 'load'
```

at `src/app/api/routes.py`, inside `generate_thesis_review`.

# Diagnosis

The advisory route is using an obsolete or ad-hoc event store API:

```python
event_store.load(aggregate_id=payload.decision_id)
```

The runtime event-store port exposes `read_events()` as the deterministic
replay read interface. Both `InMemoryEventStore` and `PostgresEventStore`
implement `read_events()`, while `PostgresEventStore` intentionally does not
expose `load()`.

The root cause is an application-layer interface mismatch, not a provider,
credential, LiteLLM, Vite proxy, or Docker networking failure.

# Minimum Correct Scope

The route should read canonical event history through `read_events()` and
filter events to the requested decision before selecting the latest thesis
artifact. This keeps advisory context reconstruction event-backed and
replayable without introducing new persistence methods.

# Invariants

- Event Ledger Canonical Truth: thesis context must come from canonical events.
- Replayability Is Foundational: reads must use deterministic event history.
- AI Advisory Boundary: generated review remains non-canonical.
- Derived State Must Remain Distinguishable: advisory output is not event truth.

# Out Of Scope

- New event-store API design.
- New lifecycle behavior.
- New advisory event types.
- LiteLLM or credential changes.
