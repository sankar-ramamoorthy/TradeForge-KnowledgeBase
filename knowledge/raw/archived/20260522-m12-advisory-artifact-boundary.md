---
title: M12 Advisory Artifact Boundary Implementation Capture
type: raw-capture
status: raw
created: 2026-05-22
tags:
  - TradeForge
  - M12
  - advisory-artifacts
  - imported-research
  - generated-advisory
  - replay-snapshot
---

# M12 Advisory Artifact Boundary Implementation Capture

## Issues

- TF-A014: Prevent automated lifecycle promotion into TradeIdea
- TF-A015: Implement candidate provenance visualization
- TF-A016: Define external research cockpit import boundary
- TF-A017: Implement research artifact ingestion API
- TF-A018: Implement Codex/Claude-generated advisory artifact support
- TF-A019: Implement advisory markdown artifact persistence
- TF-A020: Implement replay-safe advisory snapshot capture

## Implementation Summary

Runtime now has a non-canonical advisory artifact boundary for imported
research, generated advisory notes, and markdown artifacts.

The implementation adds:

- `AdvisoryArtifact` domain model and artifact query/store contracts
- in-memory and Postgres advisory artifact stores
- Alembic migration for `advisory_artifacts`
- `/advisory/artifacts` create/read/list APIs
- artifact boundary validation against lifecycle authority, execution authority,
  canonical recommendation claims, and active markdown script content
- replay-safe artifact snapshots with metadata copy, source reference count,
  caveat count, captured timestamp, and body SHA-256 digest
- explicit operator intent requirement when promoting advisory candidates into
  the existing trade idea workflow
- expanded candidate provenance details in the Opportunity Workspace

## Verification

- `uv run pytest tests\test_advisory_candidate.py tests\test_advisory_artifact.py`
- `uv run pytest`
- `uv run mypy src\domain\advisory src\services\advisory src\infrastructure\advisory src\app\api tests\test_advisory_candidate.py tests\test_advisory_artifact.py`
- `uv run ruff check --select I,F,B,UP src\app\api\application.py src\app\api\routes.py src\domain\advisory src\services\advisory src\infrastructure\advisory tests\test_advisory_candidate.py tests\test_advisory_artifact.py`
- `uv run ruff check src\domain\advisory\artifact.py src\services\advisory\artifact.py src\infrastructure\advisory\in_memory_artifact_store.py src\infrastructure\advisory\postgres_artifact_store.py tests\test_advisory_artifact.py tests\test_advisory_candidate.py`
- `npm.cmd run build`

## Notes

Artifact persistence remains outside `event_ledger`. Artifact snapshots support
historical inspection, but canonical event truth remains limited to factual
event records.
