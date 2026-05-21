---
title: TF-0068 Implementation - Advisory Provenance Tracking
type: raw-implementation-capture
status: raw
created: 2026-05-19
issue: TF-0068
milestone: M11
---

# TF-0068 Implementation - Advisory Provenance Tracking

Implemented non-canonical advisory provenance tracking:

- `AdvisoryProvenanceRecord`
- `AdvisoryProvenanceStore` protocol
- `InMemoryAdvisoryProvenanceStore`
- `AdvisoryProvenanceService`

Records preserve advisory response content, provider/model provenance, uncertainty, source references, artifact kind, and recording time.

No event ledger append path, lifecycle mutation, API endpoint, frontend surface, or Postgres persistence was introduced.

Verification:

- `uv run pytest tests\test_advisory_provenance.py tests\test_review_advisory_service.py tests\test_replay_advisory_service.py tests\test_ai_advisory_interfaces.py`
- `uv run ruff check src\domain\advisory src\services\advisory src\infrastructure\advisory tests\test_ai_advisory_interfaces.py tests\test_replay_advisory_service.py tests\test_review_advisory_service.py tests\test_advisory_provenance.py`
- `uv run mypy src\domain\advisory src\services\advisory src\infrastructure\advisory tests\test_ai_advisory_interfaces.py tests\test_replay_advisory_service.py tests\test_review_advisory_service.py tests\test_advisory_provenance.py`

