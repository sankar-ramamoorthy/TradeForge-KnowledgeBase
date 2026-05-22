---
title: Provider Provenance Tracking
type: processed-knowledge
status: processed
milestone: M9
issue: TF-0050
source_history:
  - knowledge/raw/20260513-tf-0050-provider-provenance-tracking-planning.md
  - knowledge/raw/20260513-tf-0050-provider-provenance-tracking-implementation.md
created: 2026-05-13
tags:
  - market-data
  - provenance
  - advisory
  - replay-integrity
  - audit
  - provider-boundary
related:
  - [[ProviderProvenance]]
  - [[MarketSnapshot]]
  - [[Market Intelligence Is Interpreted Context]]
  - [[Provider Boundary]]
  - [[ADR-0032]]
---

# Provider Provenance Tracking

## What It Is

The provider provenance registry is an append-only advisory audit trail that captures all market data fetch interactions during a runtime session.

It is distinct from `ProviderProvenance` (the per-snapshot record) — the registry is the *aggregate* store of all fetch events across all symbols and providers.

## Architecture

### Domain Port: `ProvenanceStore`
Structural Protocol (consistent with `EventStore`, `MarketDataProvider` patterns).
Two operations: `record_fetch()` (append) and `get_records()` (filtered reads).

### Infrastructure: `InMemoryProvenanceStore`
Session-scoped in-memory implementation. Persistent storage deferred to TF-0052.

### Domain Value Object: `ProviderFetchRecord`
Immutable. Captures: provider_id, provider_version, symbol, fetched_at, outcome (success/failure), data_as_of (success only), error_reason (failure only). Always advisory.

Factory methods: `for_success()`, `for_failure()` — enforced validation ensures semantic correctness.

### Service Integration
`MarketSnapshotService` accepts optional `provenance_store`. When set, records each fetch outcome automatically. No behavior change when not provided.

### Query Surface
`ProvenanceQueryService` wraps `ProvenanceStore` for read-only queries. Returns `ProvenanceQueryResult` with summary stats (total, success, failure counts, providers_seen, symbols_seen). Authority always ADVISORY.

### API Endpoint
`GET /provenance/market-data` — optional filters: since, until, provider_id, symbol. Returns advisory audit records for the current session.

## Design Invariants

- ProvenanceStore must NEVER write to the event ledger
- All records are advisory — not canonical facts
- Failure records are first-class (capturing what was attempted but unavailable)
- in_advisory is always True on all provenance objects

## Replay Integrity

The `ProviderFetchRecord.data_as_of` field preserves the market moment the data represents — not just when it was fetched. This supports replay integrity: "what market context was visible at decision time?"

Persistent replay-compatible storage is TF-0052 scope.
