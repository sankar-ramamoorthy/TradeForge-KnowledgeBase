---
title: TF-F071 Synthesis - Advisory Thesis Review Event Store Loading
type: processed-synthesis
status: processed
tags: [TradeForge, TF-F071, feedback, advisory, event-store]
created: 2026-05-25
updated: 2026-05-25
source_history:
  - knowledge/raw/Generate-advisory-resulting-in error.md
  - knowledge/raw/20260525-tf-f071-diagnosis.md
  - knowledge/raw/20260525-tf-f071-planning.md
related:
  - "[[Event Ledger Canonical Truth]]"
  - "[[Replayability Is Foundational]]"
  - "[[AI Advisory Boundary]]"
  - "[[Derived State Must Remain Distinguishable]]"
issues:
  - TF-F071
---

# TF-F071 Synthesis - Advisory Thesis Review Event Store Loading

## Feedback Signal

After TF-F070 made advisory invocation explicit, the operator could reach
`POST /advisory/thesis-review`, but the backend returned `500 Internal Server
Error` before LiteLLM invocation. Runtime logs showed:

```text
AttributeError: 'PostgresEventStore' object has no attribute 'load'
```

## Diagnosis

The route used an obsolete or ad-hoc event-store API:

```python
event_store.load(aggregate_id=payload.decision_id)
```

The canonical runtime `EventStore` port exposes `read_events()` for
deterministic replay reads. Both in-memory and Postgres event stores implement
that method. The failure was therefore an app-layer interface mismatch, not a
provider, LiteLLM, credential, Vite proxy, or Docker networking issue.

## Implementation Outcome

`generate_thesis_review` now reads event history through `read_events()` and
filters thesis events by the requested decision reference. It reconstructs the
latest structured thesis artifact from canonical event payloads and passes that
context to the existing thesis review advisory service.

The endpoint still returns `404` when no thesis artifact exists for the
requested decision. It does not append events, transition lifecycle state, or
make advisory output canonical.

## Invariants

- Event Ledger Canonical Truth: thesis context is derived from event history.
- Replayability Is Foundational: reconstruction uses deterministic event reads.
- AI Advisory Boundary: generated review remains advisory-only.
- Derived State Must Remain Distinguishable: advisory output is non-canonical.
- Human Decision Sovereignty: operator invocation and acceptance remain explicit.

## Verification

Completed verification:

```text
uv run pytest tests/test_advisory_thesis_review_api.py tests/test_provider_governance_api.py tests/test_default_advisory_provider_bootstrap.py
uv run mypy src\app\api\routes.py tests\test_advisory_thesis_review_api.py
uv run ruff check tests\test_advisory_thesis_review_api.py
npm.cmd run typecheck
npm.cmd run build
git diff --check
```

Focused regression coverage injects an event store with only `append()` and
`read_events()`, ensuring the route cannot regress to the old `.load()` API
assumption.

## Processing Findings

No new entity, topic, workflow, playbook, or ADR promotion is required. The
issue reinforces existing event-store and advisory boundaries.
