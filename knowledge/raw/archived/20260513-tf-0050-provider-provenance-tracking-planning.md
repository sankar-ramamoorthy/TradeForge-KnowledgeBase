---
title: TF-0050 Provider Provenance Tracking — Planning Capture
type: raw-planning-capture
issue: TF-0050
milestone: M9
status: raw
created: 2026-05-13
---

# TF-0050: Provider Provenance Tracking — Planning Capture

## Goal

Build a provider provenance registry that captures an audit trail of all market data fetch interactions.

Existing `ProviderProvenance` on `MarketSnapshot` is a per-snapshot record.
TF-0050 is the aggregate registry capturing all fetch interactions over the process lifetime.

## Problem Statement

When a decision is made, TradeForge needs to answer:
"What market data was fetched, from which provider, at what time, and was it available?"

This supports:
- Replay integrity: what market context was visible at decision time
- Provider auditability: which providers were used and when
- Partial failure visibility: what data was attempted but unavailable

## Design Decisions

### No New ADR Required

ADR-0032 already establishes provenance tracking as an architectural requirement:
> "Provenance tracking preserves replay integrity. Historical reconstructions must be able to reference which provider and version supplied the context visible at the time of a decision."

TF-0050 implements that requirement. The architectural model is already established.

### ProvenanceStore Is Separate From EventStore

Provenance records are advisory artifacts, not canonical facts.
The ProvenanceStore must never write to the event ledger.
This is a separate advisory audit trail — a distinct port from EventStore.

### In-Memory For M9

Persistent provenance storage is addressed in TF-0052 (replay-compatible market snapshot persistence).
M9 scope: InMemoryProvenanceStore is sufficient for session-scoped tracking.

### Integration Point: MarketSnapshotService

Rather than requiring callers to record provenance manually, integrate recording directly into MarketSnapshotService.
Optional dependency injection: `provenance_store: ProvenanceStore | None = None`.
When None: behavior unchanged (no breaking changes to existing tests or consumers).
When set: auto-records each fetch outcome.

### Failure Records Are First-Class

The registry captures failures as well as successes.
A failure record explains: "we attempted to fetch AAPL from yfinance at T, and it failed because X."
This is important for replay integrity — knowing that data was unavailable at a moment is itself a fact.

## Affected Layers

- Domain: `ProviderFetchRecord`, `ProvenanceStore` Protocol
- Infrastructure: `InMemoryProvenanceStore`
- Services: `MarketSnapshotService` (optional integration), `ProvenanceQueryService`
- App: `GET /provenance/market-data` endpoint
- Tests: `tests/test_provider_provenance.py`

## Invariants Impacted

- Layer Separation: ProvenanceStore is domain Protocol; InMemoryProvenanceStore is infrastructure
- Market Intelligence Is Interpreted Context: provenance records are advisory, not canonical
- Derived State Must Remain Distinguishable: provenance records are advisory artifacts
- Event Integrity: provenance store never writes to event ledger

## Replay Implications

- Provenance records are not replayed like lifecycle events — they are session-scoped audit artifacts
- TF-0052 will address persistent replay-compatible storage
- M9: in-memory only, no replay dependency

## Testing Strategy

- Unit tests for ProviderFetchRecord validation
- Unit tests for InMemoryProvenanceStore (append, query, filter)
- Integration tests for MarketSnapshotService with provenance store
- API endpoint tests for GET /provenance/market-data

## Unresolved

- Whether the API should paginate large provenance logs (deferred — M9 scope is lightweight)
- Whether provenance records should be queryable from the Replay Workspace (TF-0052 territory)
