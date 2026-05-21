---
title: TF-0052 Synthesis — Replay-Compatible Market Snapshot Persistence
type: processed-synthesis
status: processed
issue: TF-0052
milestone: M9
created: 2026-05-13
source_history:
  - knowledge/raw/20260513-tf-0052-snapshot-persistence-planning.md
  - knowledge/raw/20260513-tf-0052-snapshot-persistence-implementation.md
tags: [market-data, persistence, replay, advisory, postgres, M9]
related:
  - "[[Provider Provenance Tracking]]"
  - "[[Replayability Is Foundational]]"
  - "[[Event Ledger Canonical Truth]]"
---

# TF-0052 Synthesis — Replay-Compatible Market Snapshot Persistence

## Stabilized Understanding

TF-0052 persists full OHLCV advisory snapshots (including provenance and regime) so historical replay can reconstruct "what market context was visible at the time of a decision." This is distinct from TF-0050 (ProvenanceStore) which records only that a fetch occurred — TF-0052 stores the actual data.

## Key Design Decisions Preserved

- `market_advisory_snapshots` Postgres table is NEVER the event ledger. Advisory data and canonical events are architecturally separated.
- Decimal prices stored as TEXT in Postgres to preserve precision (consistent with JSON API representation).
- Two infrastructure implementations: `InMemoryMarketSnapshotStore` (session-scoped) and `PostgresMarketSnapshotStore` (production). Composition root injects the appropriate one.
- `MarketSnapshotService` integrates persistence optionally via `snapshot_persistence_store: MarketSnapshotPersistenceStore | None = None` — no behavior change when not set (backward compatible).
- Indices on `(symbol, fetched_at)` and `(provider_id, fetched_at)` for replay queries.

## Critical Replay Field

`ProviderProvenance.data_as_of` is the temporal anchor for replay — it records what market moment the data represents, not when it was fetched. Without this distinction, historical reconstruction cannot determine what market context existed at a decision point.

## Invariant

`is_advisory` is always True on all persisted records. Advisory data never becomes canonical.
