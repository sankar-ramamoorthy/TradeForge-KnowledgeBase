---
title: TF-0052 Replay-Compatible Market Snapshot Persistence — Implementation Capture
type: raw-implementation-capture
issue: TF-0052
milestone: M9
status: raw
created: 2026-05-13
---

# TF-0052: Replay-Compatible Market Snapshot Persistence — Implementation Capture

## What Was Built

### Domain Layer: `src/domain/market/snapshot_persistence.py`
- `PersistedMarketSnapshot` — immutable frozen dataclass; wraps MarketSnapshot with snapshot_id
  and persisted_at; is_advisory/symbol/provider_id properties
- `MarketSnapshotPersistenceStore` — structural Protocol port; `persist()` + `get_snapshots()`
  with optional since/until/provider_id/symbol filters

### Infrastructure Layer: `src/infrastructure/market/in_memory_snapshot_store.py`
- `InMemoryMarketSnapshotStore` — auto-incrementing snapshot_id; filters by persisted_at
  (not fetched_at like ProvenanceStore — snapshots are stored when written, not when fetched)
- Satisfies MarketSnapshotPersistenceStore Protocol structurally

### Infrastructure Layer: `src/infrastructure/market/postgres_snapshot_store.py`
- `PostgresMarketSnapshotStore` — Postgres table `market_advisory_snapshots`; sa.Table + sa.Engine
  pattern consistent with PostgresEventStore; Decimal prices as TEXT; regime as TEXT with UNKNOWN
  fallback on deserialization; UTC normalization on read
- `_sqlalchemy_url` converts `postgresql://` to `postgresql+psycopg://`

### Migration: `migrations/versions/20260513_0003_create_market_snapshots.py`
- Table `market_advisory_snapshots` with explicit advisory comment
- Indices: `ix_market_advisory_snapshots_symbol_fetched` and `_provider_fetched`
- Chains from `20260511_0002` (event ledger migration)
- Full downgrade support

### Services Layer: `src/services/market/snapshot_service.py` (updated)
- Added optional `snapshot_persistence_store: MarketSnapshotPersistenceStore | None = None`
- `_persist_snapshot()` private method — silent failure (persistence never breaks fetch)
- Called after `_record_success()` in both `fetch_context` and `fetch_snapshot`

### Services Layer: `src/services/market/snapshot_query.py` (new)
- `MarketSnapshotQueryAuthority` StrEnum (ADVISORY only)
- `MarketSnapshotQueryResult` — immutable, always advisory
- `MarketSnapshotQueryService` — stateless read-only wrapper over the store

### App Layer: `src/app/api/routes.py` (updated)
- `PersistedMarketSnapshotResponse` and `MarketSnapshotQueryResponse` Pydantic models
- `market_router = APIRouter(prefix="/market", tags=["market"])`
- `GET /market/snapshots` endpoint with optional since/until/provider_id/symbol filters
- Registered via `runtime_router.include_router(market_router)`

### App Layer: `src/app/api/application.py` (updated)
- `InMemoryMarketSnapshotStore` shared between `MarketSnapshotService` (write) and
  `MarketSnapshotQueryService` (read)
- `market_snapshot_query_service` injectable param for test overrides

## Test Coverage: 45 tests
- `TestPersistedMarketSnapshot`: 4 tests (advisory, symbol/provider delegation, immutability)
- `TestInMemoryMarketSnapshotStore`: 12 tests (append, query, filter, ordering, Protocol)
- `TestMarketSnapshotServicePersistenceIntegration`: 8 tests (success/failure, tolerance, regime)
- `TestMarketSnapshotQueryService`: 5 tests (query, authority, filter, empty)
- `TestPostgresMarketSnapshotStore`: 8 tests (shape, URL, migration structure)
- `TestMarketSnapshotsEndpoint`: 8 tests (empty, persisted, filters, live fetch→endpoint)

## Key Design Decisions

### Silent Persistence Failure
`_persist_snapshot()` catches all exceptions silently. Market data fetch must never fail
due to a storage error. This is the advisory tolerance pattern used throughout M9.

### persisted_at vs fetched_at
InMemoryMarketSnapshotStore filters by `persisted_at` (when written to store).
ProvenanceStore filters by `fetched_at` (when the provider was called).
This distinction matters for replay: persisted_at is the local write time;
fetched_at is the authoritative provider timestamp.

### Table Name
`market_advisory_snapshots` — explicit "advisory" in the name reinforces the separation
from `event_ledger` and makes accidental canonical use visually obvious.

## Verification
- `uv run pytest tests/test_market_snapshot_persistence.py` — 45 passed
- `uv run pytest` — 493 passed
- `uv run ruff check .` — clean
- `uv run mypy src tests` — clean (97 files)
