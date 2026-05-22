---
title: TF-A008 Through TF-A010 Advisory Observation Closeout Capture
type: raw-implementation-capture
status: archived
created: 2026-05-21
tags: [tf-a008, tf-a009, tf-a010, m12, advisory-observation, runtime-closeout]
---

# TF-A008 Through TF-A010 Advisory Observation Closeout Capture

## Issues

- TF-A008: Implement contextual interpretation artifacts.
- TF-A009: Implement conflicting evidence surfacing.
- TF-A010: Implement evidence aging/staleness visibility.

Milestone: M12 - Advisory Observation And Cognitive Evidence Layer.

Runtime branch: `feature/m12-advisory-observation-foundation`.

## Design Boundary

These issues were implemented as M12 advisory observation metadata, not M13
`AdvisoryInterpretation` semantics.

The implementation excludes:

- thesis influence
- contextual weight
- advisory confidence range
- buy/sell direction
- scoring or ranking
- recommendation authority
- lifecycle transition intent
- execution authority

## TF-A008 Result

Added `ContextualObservationArtifact` as lightweight non-canonical context
attached to `AdvisoryObservation`.

Contextual artifacts can preserve:

- regime notes
- market context references
- source links
- provenance summary
- caveats

Contextual artifact content persists in the non-canonical advisory observation
artifact store and is excluded from canonical capture event payloads.

## TF-A009 Result

Added qualitative conflict visibility for advisory observations.

Conflict markers can come from:

- explicit `EvidenceConflictMarker` metadata on evidence records
- operator caveats containing conflict/contradiction language

API responses expose conflict markers as advisory metadata. They do not rank,
score, approve, reject, promote, or derive thesis influence.

## TF-A010 Result

Added derived evidence staleness visibility in advisory observation API
responses.

Staleness is computed from persisted evidence timestamps and the observation
capture timestamp. It is derived advisory metadata only and does not mutate,
delete, rewrite, or downgrade historical evidence.

## Validation

Focused tests:

```text
uv run pytest tests\test_advisory_observation.py tests\test_replay_timeline.py tests\test_replay_timeline_service.py tests\test_advisory_interpretation.py
```

Result:

```text
34 passed, 1 warning
```

Focused ruff checks:

```text
uv run ruff check src\domain\advisory\observation.py src\domain\advisory\__init__.py src\infrastructure\advisory\postgres_observation_store.py tests\test_advisory_observation.py migrations\versions\20260521_0006_add_contextual_observation_artifacts.py
```

Result:

```text
All checks passed.
```

The warning was from the third-party `websockets.legacy` package.

## Operational Sync

`DOCS/ISSUE_REGISTER.md` was updated:

- TF-A008 marked Done.
- TF-A009 marked Done.
- TF-A010 marked Done.

