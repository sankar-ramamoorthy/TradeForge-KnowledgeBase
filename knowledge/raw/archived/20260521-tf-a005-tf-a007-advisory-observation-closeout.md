---
title: TF-A005 Through TF-A007 Advisory Observation Closeout Capture
type: raw-implementation-capture
status: archived
created: 2026-05-21
tags: [tf-a005, tf-a006, tf-a007, m12, advisory-observation, runtime-closeout]
---

# TF-A005 Through TF-A007 Advisory Observation Closeout Capture

## Issues

- TF-A005: Implement replay-visible advisory observation timeline.
- TF-A006: Implement evidence attachment framework.
- TF-A007: Implement thesis evidence linkage.

Milestone: M12 - Advisory Observation And Cognitive Evidence Layer.

Runtime branch: `feature/m12-advisory-observation-foundation`.

## TF-A005 Result

Replay-visible advisory observation capture facts were verified and strengthened with focused tests.

Runtime behavior:

- `advisory.observation_captured` events become advisory replay timeline entries.
- Timeline ordering is deterministic by timestamp and source sequence.
- Replay entries preserve artifact ID and advisory/non-canonical boundary metadata.
- Replay payloads exclude advisory content.
- Replay reconstruction does not depend on current AI output or live provider calls.

## TF-A006 Result

The evidence attachment framework was extended.

Runtime behavior:

- `CognitiveEvidence` supports M12 evidence source kinds:
  - provider payload
  - imported research
  - markdown artifact
  - URL
  - operator note
  - replay annotation
  - generated advisory artifact
- Evidence records can preserve source URI, artifact ID, captured timestamp, provenance summary, and caveats.
- Evidence detail persists in the non-canonical advisory observation artifact store.
- Canonical capture events retain only source-reference facts and exclude evidence summary, provenance detail, caveats, and advisory content.

## TF-A007 Result

Contextual decision/thesis linkage was implemented with deterministic validation.

Runtime behavior:

- Advisory observations may reference an existing decision and/or thesis.
- The capture service validates optional `decision_id` and `thesis_id` against event-store history before persistence.
- Invalid decision/thesis links are rejected.
- Valid links remain contextual advisory metadata in API responses and replay payloads.
- Linkage does not create thesis influence, thesis revision, lifecycle transition, plan change, approval, or execution authority.

## Validation

Focused tests:

```text
uv run pytest tests\test_advisory_observation.py tests\test_replay_timeline.py tests\test_replay_timeline_service.py
```

Result:

```text
25 passed, 1 warning
```

Ruff:

```text
uv run ruff check src\domain\advisory\contracts.py src\domain\advisory\observation.py src\services\advisory\observation.py src\infrastructure\advisory\postgres_observation_store.py tests\test_advisory_observation.py tests\test_replay_timeline.py
```

Result:

```text
All checks passed.
```

Note: running ruff against the entire `src/app/api/routes.py` file still reports pre-existing lint issues outside the touched M12 advisory observation changes.

## Operational Sync

`DOCS/ISSUE_REGISTER.md` was updated:

- TF-A005 marked Done.
- TF-A006 marked Done.
- TF-A007 marked Done.

