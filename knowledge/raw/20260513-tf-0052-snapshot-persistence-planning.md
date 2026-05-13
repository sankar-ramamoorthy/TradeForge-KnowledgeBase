---
title: TF-0052 Replay-Compatible Market Snapshot Persistence — Planning Capture
type: raw-planning-capture
issue: TF-0052
milestone: M9
status: raw
created: 2026-05-13
---

# TF-0052: Replay-Compatible Market Snapshot Persistence — Planning Capture

## Goal

Persist full advisory market snapshots (OHLCV + provenance + regime) durably so
historical replay can reconstruct "what market context was visible at the time of
a decision" — not just that a fetch occurred (TF-0050), but what the actual data said.

## Distinction from TF-0050

ProvenanceStore (TF-0050): lightweight audit — "was fetched, when, success/fail"
SnapshotPersistenceStore (TF-0052): full OHLCV archive — "what was the data"

Both are advisory. Neither writes to the event ledger.

## Design

### Domain Port: `MarketSnapshotPersistenceStore`
Structural Protocol (consistent with ProvenanceStore, EventStore, MarketDataProvider).
- `persist(snapshot)` → None (append-only)
- `get_snapshots(since, until, provider_id, symbol)` → tuple[PersistedMarketSnapshot, ...]

### Domain Value: `PersistedMarketSnapshot`
Wraps a `MarketSnapshot` with persistence metadata (snapshot_id: int, persisted_at: datetime).
Always advisory.

### Infrastructure: `InMemoryMarketSnapshotStore`
Session-scoped. Auto-incrementing snapshot_id counter.

### Infrastructure: `PostgresMarketSnapshotStore`
Postgres table `market_advisory_snapshots` — separate from `event_ledger`.
Uses sa.Table + sa.Engine pattern (consistent with PostgresEventStore).
Decimal prices stored as TEXT for precision preservation (consistent with JSON API).
Indices on (symbol, fetched_at) and (provider_id, fetched_at) for replay queries.

### Migration: `20260513_0003_create_market_snapshots.py`
No append-only trigger — advisory data, not canonical facts.
Table comment documents advisory-only purpose.

### Services: `MarketSnapshotService` integration
Optional `snapshot_persistence_store: MarketSnapshotPersistenceStore | None = None`.
Persist on success; failures are silent (advisory tolerance pattern).

### Services: `MarketSnapshotQueryService`
Read-only query service. Returns `MarketSnapshotQueryResult` (advisory).

### App: `GET /market/snapshots`
New `market_router` (prefix="/market"). Optional filters: since, until, provider_id, symbol.

### App wiring
`create_app()` gets optional `market_snapshot_query_service` param.
Default: `InMemoryMarketSnapshotStore` shared between service and query service.
Production: inject `PostgresMarketSnapshotStore`.

## ADR Impact

None — ADR-0032 already establishes replay-compatible persistence as a requirement.

## Invariants

- Event Integrity: market_advisory_snapshots table is NEVER the event ledger
- Derived State Must Remain Distinguishable: `is_advisory=True` on all persisted records
- Replay: fetched_at + data_as_of preserved for temporal reconstruction
- Layer Separation: PostgresMarketSnapshotStore is infrastructure; port is domain
