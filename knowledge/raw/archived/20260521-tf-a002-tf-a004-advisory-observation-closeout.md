---
title: TF-A002 Through TF-A004 Advisory Observation Closeout Capture
type: raw-implementation-capture
status: archived
created: 2026-05-21
tags: [tf-a002, tf-a003, tf-a004, m12, advisory-observation, runtime-closeout]
---

# TF-A002 Through TF-A004 Advisory Observation Closeout Capture

## Issues

- TF-A002: Implement advisory observation event taxonomy.
- TF-A003: Implement observation provenance persistence.
- TF-A004: Implement uncertainty metadata support.

Milestone: M12 - Advisory Observation And Cognitive Evidence Layer.

Runtime branch: `feature/m12-advisory-observation-foundation`.

## Runtime Loop Finding

The runtime already contained most implementation required for TF-A002 through TF-A004. The closeout pass verified existing code and added a narrow regression test for invalid uncertainty-band handling.

## TF-A002 Result

Verified:

- `EventDomain.ADVISORY` exists.
- `advisory.observation_captured` is appended by `AdvisoryObservationCaptureService`.
- Capture payload records capture facts only.
- Payload excludes observation content, recommendation authority, lifecycle transition intent, and execution authority.
- Replay timeline can expose the advisory capture fact without advisory content.

Validation:

```text
uv run pytest tests\test_advisory_observation.py::test_advisory_event_domain_and_capture_event_are_supported tests\test_advisory_observation.py::test_replay_timeline_includes_advisory_capture_fact_without_content tests\test_domain_events.py
```

Result:

```text
9 passed, 1 warning
```

## TF-A003 Result

Verified:

- `InMemoryAdvisoryObservationStore` exists for tests/default runtime behavior.
- `PostgresAdvisoryObservationStore` exists for durable non-canonical artifact persistence.
- `advisory_observations` table is separate from `event_ledger`.
- Stored records include content, evidence, provenance summary, caveats, tags, capture origin, persona/workspace context, optional decision/thesis context, uncertainty band, and captured timestamp.
- Records are retrievable by observation ID and listable with required filters.

Validation:

```text
uv run pytest tests\test_advisory_observation.py::test_in_memory_observation_store_persists_and_filters tests\test_advisory_observation.py::test_postgres_observation_store_shape_and_migration tests\test_advisory_observation.py::test_create_read_list_api_labels_advisory_observations
```

Result:

```text
3 passed, 1 warning
```

## TF-A004 Result

Verified and strengthened:

- `AdvisoryUncertaintyBand` supports `low`, `medium`, `high`, and `unknown`.
- Caveats are required.
- API responses expose uncertainty and caveats as advisory metadata.
- Added tests that invalid uncertainty bands fail validation through the domain enum and API boundary.

Validation:

```text
uv run pytest tests\test_advisory_observation.py
```

Result:

```text
10 passed, 1 warning
```

The warning was from the third-party `websockets.legacy` package and did not affect these issues.

## Operational Sync

`DOCS/ISSUE_REGISTER.md` was updated:

- TF-A002 marked Done with implementation summary and validation.
- TF-A003 marked Done with implementation summary and validation.
- TF-A004 marked Done with implementation summary and validation.

Runtime code changes were limited to tests for TF-A004 uncertainty validation.

