---
title: M12 Advisory Candidate Workflow Implementation Capture
type: raw-capture
status: raw
created: 2026-05-22
tags:
  - TradeForge
  - M12
  - advisory-candidates
  - candidate-review-queue
  - lifecycle-boundary
---

# M12 Advisory Candidate Workflow Implementation Capture

## Issues

- TF-A011: Implement advisory candidate ingestion pipeline
- TF-A012: Implement candidate review queue
- TF-A013: Implement operator candidate promotion workflow

## Planning Summary

The implementation preserves ADR-0041 by modeling advisory candidates as
non-canonical advisory artifacts backed by advisory observation persistence.
Candidate ingestion appends only `advisory.observation_captured` and does not
create trade ideas, thesis artifacts, plans, approvals, execution state, or
decision lifecycle transitions.

The candidate review queue is a derived read model scoped by persona and
workspace. Its ordering is deterministic: newest captured candidate first, then
candidate ID. Dismissal is view/session behavior only and is not persisted as
canonical state.

Operator promotion remains lifecycle-owned. Advisory candidate content can
prefill the existing new trade idea modal, but the operator may edit it and must
explicitly submit the lifecycle workflow. The resulting event is a normal
human-owned `decision.trade_idea_created` event with advisory traceability only.

## Implementation Summary

- Added `AdvisoryCandidate` domain view over advisory observations.
- Added advisory candidate ingestion/query services and a derived candidate
  review queue service.
- Added `/advisory/candidates` and `/advisory/candidates/review-queue` APIs.
- Extended `/lifecycle/decisions/init` with optional advisory candidate source
  validation and traceability.
- Added Opportunity Workspace candidate queue UI with inspect, dismiss, and
  begin-workflow actions.
- Extended the new trade idea modal with editable advisory prefill support.

## Verification

- `uv run pytest tests\test_advisory_candidate.py tests\test_advisory_observation.py`
- `uv run pytest`
- `uv run mypy src\domain\advisory src\services\advisory src\app\api tests\test_advisory_candidate.py`
- `uv run ruff check src\domain\advisory\candidate.py src\services\advisory\candidate.py tests\test_advisory_candidate.py`
- `uv run ruff check --select I,B008 src\app\api\routes.py src\domain\advisory\__init__.py`
- `npm.cmd run build`

## Follow-Up Notes

TF-A014 should add explicit negative safeguards around automated lifecycle
promotion attempts from advisory ingestion paths. This implementation avoids
such a path but does not replace that future guardrail issue.
