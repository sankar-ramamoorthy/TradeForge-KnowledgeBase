---
title: TF-0065 Implementation - AI Advisory Interfaces
type: raw-implementation-capture
status: raw
created: 2026-05-19
issue: TF-0065
milestone: M11
---

# TF-0065 Implementation - AI Advisory Interfaces

## Runtime Changes

- Added pure domain advisory contracts under `src/domain/advisory/`.
- Added provider-agnostic `AIAdvisoryProvider` protocol.
- Added immutable advisory request, response, provenance, uncertainty, and source-reference models.
- Added services-layer `AIAdvisoryService` that invokes a provider and validates response identity, artifact kind, and advisory authority.
- Added `DOCS/ai-advisory-interfaces.md` as the implementation contract for later M11 issues.
- Added focused tests for authority, immutability, validation, provider orchestration, and boundary imports.

## Event / Replay / Lifecycle Notes

- No events were added.
- No event store dependency was introduced.
- No lifecycle transition authority was introduced.
- Advisory responses preserve source references and provenance for later replay/provenance work, but TF-0065 does not persist advisory artifacts.

## ADR Notes

No new ADR was created. ADR-0006 directly governs this boundary.

## Verification

- `uv run pytest tests\test_ai_advisory_interfaces.py`
- `uv run ruff check src\domain\advisory src\services\advisory tests\test_ai_advisory_interfaces.py`
- `uv run mypy src\domain\advisory src\services\advisory tests\test_ai_advisory_interfaces.py`
- `uv run pytest` - 699 passed with local `.keys.enc` temporarily hidden during the test process and restored afterward.

## Regression Note

Broad `uv run ruff check src tests` still reports pre-existing lint debt in older runtime modules unrelated to TF-0065. The TF-0065 files are ruff-clean under focused checking.
