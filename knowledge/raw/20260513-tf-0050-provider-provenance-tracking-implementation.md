---
title: TF-0050 Provider Provenance Tracking — Implementation Capture
type: raw-implementation-capture
issue: TF-0050
milestone: M9
status: raw
created: 2026-05-13
---

# TF-0050: Provider Provenance Tracking — Implementation Capture

## What Was Built

### Domain Layer: `src/domain/market/provenance.py`
- `ProviderFetchRecord` — immutable frozen dataclass; factory methods `for_success` and `for_failure`; `is_advisory` property always returns True
- `ProvenanceStore` — structural Protocol port; `record_fetch()` + `get_records()` with optional since/until/provider_id/symbol filters

### Infrastructure Layer: `src/infrastructure/market/in_memory_provenance_store.py`
- `InMemoryProvenanceStore` — satisfies ProvenanceStore Protocol structurally; records held in memory list; `get_records()` filters and returns sorted by `fetched_at` ascending

### Services Layer: `src/services/market/snapshot_service.py` (updated)
- Added optional `provenance_store: ProvenanceStore | None = None` param
- Added `_record_success()` and `_record_failure()` private methods
- `fetch_context()` records each symbol outcome (success or failure)
- `fetch_snapshot()` records outcome and re-raises on failure
- No behavior change when provenance_store is None (backward compatible)

### Services Layer: `src/services/market/provenance_query.py` (new)
- `ProvenanceQueryAuthority` StrEnum (ADVISORY only)
- `ProvenanceQueryResult` — immutable result with records, counts, providers_seen, symbols_seen
- `ProvenanceQueryService` — stateless query service; wraps ProvenanceStore; computes summary stats

### App Layer: `src/app/api/routes.py` (updated)
- Added `ProviderFetchRecordResponse` and `ProvenanceQueryResponse` Pydantic models
- Added `provenance_router = APIRouter(prefix="/provenance")`
- Added `_provenance_query_service_from()` helper
- Added `GET /provenance/market-data` endpoint with optional since/until/provider_id/symbol filters
- Registered `provenance_router` via `runtime_router.include_router`

### App Layer: `src/app/api/application.py` (updated)
- `InMemoryProvenanceStore` shared between `MarketSnapshotService` (writes) and `ProvenanceQueryService` (reads)
- Single store instance ensures reads reflect all fetch interactions from the session
- Added `provenance_query_service` injectable param for test overrides

## Test Coverage: 39 tests
- `TestProviderFetchRecord`: 7 tests (factory methods, validation, immutability)
- `TestInMemoryProvenanceStore`: 9 tests (append, query, filter combinations, protocol satisfaction)
- `TestMarketSnapshotServiceProvenanceIntegration`: 7 tests (success/failure recording, mixed outcomes, backward compat)
- `TestProvenanceEndpoint`: 8 tests (empty registry, success/failure records, filters, default app)

## Verification
- `uv run pytest tests/test_provider_provenance.py` — 39 passed
- `uv run pytest` — 415 passed
- `uv run ruff check .` — clean
- `uv run mypy src tests` — clean (90 files)

## Replay Implications
- In-memory store: provenance records do not survive process restart
- Persistent provenance storage is TF-0052 scope
- The `ProviderFetchRecord.for_success` captures `fetched_at` and `data_as_of` from the snapshot's own provenance, preserving the original provider timestamps

## Architectural Observations
- Protocol-based port (ProvenanceStore) is consistent with EventStore and MarketDataProvider patterns
- Shared store instance between MarketSnapshotService (write) and ProvenanceQueryService (read) enforces a single advisory audit channel per application instance
- The `since: datetime | None = None` default (no `Query()` wrapper) avoids B008 ruff rule for FastAPI datetime query params — simpler than `Query(default=None)`

## Unresolved
- Pagination for large provenance logs (deferred — M9 scope is lightweight)
- Provenance integration with Replay Workspace UI (TF-0052 territory)
- Provider health dashboard surface (future consideration)
