---
title: TF-0043 Normalized Market Snapshot Model — Implementation Capture
type: raw-implementation
status: raw
created: 2026-05-13
issue: TF-0043
milestone: M9
---

# TF-0043: Normalized Market Snapshot Model — Implementation Capture

## Implementation Summary

Created `src/services/market/` with three files:
- `context.py` — MarketContextAuthority, MarketContextRequest, SymbolFetchResult, MarketContextResult
- `snapshot_service.py` — MarketSnapshotService
- `__init__.py` — public re-exports

One test file:
- `tests/test_market_snapshot_service.py` — 36 tests, all passing

## Entities Introduced

### MarketContextAuthority (StrEnum)
ADVISORY only. No code path can produce a non-advisory result.

### MarketContextRequest (frozen dataclass)
symbols tuple + optional persona_id. Validates non-empty symbols.
persona_id reserved for future persona-shaped context weighting (M10).

### SymbolFetchResult (frozen dataclass)
Per-symbol discriminated union: success carries MarketSnapshot, failure carries error_reason.
Invariant: exactly one of snapshot / error_reason must be set.
Factory methods: SymbolFetchResult.success(snap), SymbolFetchResult.failure(symbol, reason).

### MarketContextResult (frozen dataclass)
Batch result: available (snapshots), unavailable_symbols, symbol_results, provider_id, fetched_at.
Three computed state properties: is_complete, is_partial, is_empty.
snapshot_for(symbol) convenience lookup.
Authority always ADVISORY.

### MarketSnapshotService (class)
fetch_context(request) — batch, never raises for individual failures, returns MarketContextResult.
fetch_snapshot(symbol) — single, propagates ProviderUnavailableError.

## Design Decisions Made During Implementation

1. fetch_context iterates symbols one-by-one rather than calling fetch_snapshots.
   Reason: per-symbol error capture. batch fetch_snapshots would complicate partial
   failure tracking since a single exception would abort the whole batch.

2. SymbolFetchResult as a discriminated union (exactly one field set).
   Reason: prevents the "success with None snapshot" or "failure with no reason" 
   ambiguous states. Validates at construction time.

3. MarketContextResult.symbol_results carries the full per-symbol record.
   Reason: workspace overlays and regime interpretation (TF-0047, TF-0048) may
   want to surface which symbols failed and why, not just that some were unavailable.

4. persona_id on MarketContextRequest is optional/None in M9.
   Reason: architectural placeholder for M10 persona-shaped market context weighting.
   Avoids a breaking interface change later.

## Test Coverage Summary

36 tests across:
- MarketContextRequest: immutability, coercion, validation (empty/blank)
- SymbolFetchResult: factories, discriminated union invariant, immutability
- MarketContextResult: authority, is_complete/is_partial/is_empty, snapshot_for, validation
- MarketSnapshotService (all-available stub): complete result, advisory authority, symbol mapping, is_advisory
- MarketSnapshotService (partial-failure stub): partial result, excluded symbols, no-raise, fetch_snapshot raises
- MarketSnapshotService (total-failure stub): empty result, all unavailable recorded, no-raise, fetch_snapshot raises

## Verification Commands

- `uv run pytest tests/test_market_snapshot_service.py` — 36 passed
- `uv run pytest` — 254 passed
- `uv run ruff check .` — clean
- `uv run mypy src tests` — clean, 73 files

## Follow-up Issues

- TF-0044: yfinance adapter implementing MarketDataProvider Protocol
- TF-0045: Polygon/Massive.com adapter
- TF-0046: Alpaca adapter
- TF-0047: Workspace context overlays consuming MarketSnapshotService
