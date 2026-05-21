---
title: TF-0042 Provider Boundary Interfaces — Implementation Capture
type: raw-implementation
status: raw
created: 2026-05-13
issue: TF-0042
milestone: M9
---

# TF-0042: Define Provider Boundary Interfaces — Implementation Capture

## Implementation Summary

Created `src/domain/market/` as the new domain market module.

Three files:
- `src/domain/market/snapshot.py` — domain value objects
- `src/domain/market/provider.py` — provider port interface + error type
- `src/domain/market/__init__.py` — public re-exports

One test file:
- `tests/test_provider_boundary.py` — 28 tests, all passing

## Entities Introduced

### MarketRegime (StrEnum)
Advisory regime classification. Defaults to UNKNOWN. Never canonical.

Values: BULL, BEAR, RANGING, HIGH_VOLATILITY, LOW_VOLATILITY, UNKNOWN

### ProviderProvenance (frozen dataclass)
Immutable record of data origin for replay integrity.
- `provider_id` — stable string identifier ("yfinance", "alpaca", "polygon")
- `provider_version` — adapter version
- `fetched_at` — when data was retrieved (real-world time)
- `data_as_of` — what market moment the data represents

The fetched_at / data_as_of distinction is the key insight: enables future
TF-0052 snapshot persistence to store what market context existed at any
historical decision point.

### PriceOHLCV (frozen dataclass)
Normalized OHLCV price record. Uses Decimal for precision.
Validates: symbol non-empty, volume >= 0, open/close within [low, high].

### MarketSnapshot (frozen dataclass)
Complete advisory snapshot = PriceOHLCV + ProviderProvenance + MarketRegime.
- `is_advisory` property always returns True — machine-readable boundary marker
- `symbol`, `provider_id`, `data_as_of` convenience properties delegate to children

### MarketDataProvider (Protocol)
Provider port interface. Structural subtyping (no inheritance required).
Consistent with EventStore Protocol pattern.
Methods: fetch_snapshot(symbol) -> MarketSnapshot, fetch_snapshots(symbols) -> tuple

### ProviderUnavailableError (Exception)
Explicit failure type. Carries provider_id, symbol, reason.
Required so callers cannot silently receive empty/stale context on failure.

## Design Decisions Made During Implementation

1. Used `Decimal` for all price fields — right for financial data even though
   it's advisory context. If any future threshold comparison logic is added,
   float would introduce precision bugs.

2. `PriceOHLCV.__post_init__` validates OHLCV invariants eagerly — prevents
   malformed provider data from propagating into workspace overlays.

3. `ProviderUnavailableError` made explicit (not just `RuntimeError`) —
   provider failures must be catchable by type in adapter orchestration.

4. Protocol structural typing confirmed by mypy — `_StubProvider` in tests
   satisfies `MarketDataProvider` without `# type: ignore`.

## Test Summary

28 tests across:
- ProviderProvenance: immutability, empty-string rejection, fetched_at/data_as_of distinction
- PriceOHLCV: immutability, OHLCV validation, Decimal type assertion
- MarketSnapshot: immutability, is_advisory=True, symbol/provider_id/data_as_of delegation,
  regime default, context_notes as tuple
- MarketRegime: string membership, value assertion
- MarketDataProvider Protocol: stub structural typing, fetch_snapshots ordering
- ProviderUnavailableError: carries structured context, is Exception

## Verification Commands

- `uv run pytest tests/test_provider_boundary.py` — 28 passed
- `uv run pytest` — 218 passed
- `uv run ruff check .` — clean
- `uv run mypy src tests` — clean, 69 files

## Replay Implications

The `ProviderProvenance.data_as_of` field is the key replay anchor.
When TF-0052 persists market snapshots, it stores this field so historical
reconstruction can determine what market data was visible at the time of
each decision — without calling live provider APIs.

## Lifecycle Implications

None. The market module introduces no lifecycle state, no events, and no
transitions. It is structurally isolated from the event ledger.

## Architectural Observations

The market domain module is cleanly isolated. It has zero imports from
lifecycle, workspace, or event modules. Services and workspace overlays
(TF-0047 onwards) will import from this module, not the reverse.

This boundary ensures providers remain subordinate to the system's
event-sourced canonical architecture.

## Follow-up Issues (M9 continuation)

- TF-0043: Implement normalized market snapshot model (data services layer)
- TF-0044: Add read-only yfinance provider adapter
- TF-0045: Add Massive.com/Polygon market data adapter
- TF-0046: Add Alpaca market data adapter
