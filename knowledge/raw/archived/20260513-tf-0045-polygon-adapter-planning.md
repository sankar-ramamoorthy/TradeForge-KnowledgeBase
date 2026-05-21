---
title: TF-0045 Planning — Polygon/Massive.com Provider Adapter
type: planning-capture
status: raw
issue: TF-0045
milestone: M9
created: 2026-05-13
---

# TF-0045 Planning: Add Massive.com (Polygon.io) Market Data Adapter

## Issue Goal

Add a read-only Polygon.io provider adapter that satisfies the `MarketDataProvider`
Protocol defined in TF-0042. This completes the second of three planned M9 provider
adapters (yfinance done in TF-0044; Alpaca follows in TF-0046).

## Affected Layer

infrastructure (`src/infrastructure/market/polygon_adapter.py`)

## Affected Modules

- `src/infrastructure/market/polygon_adapter.py` — new adapter class
- `tests/test_polygon_adapter.py` — new test suite (~20 tests, all mocked)
- `pyproject.toml` — add `polygon-api-client>=1.0` dependency + mypy override
- `DOCS/ISSUE_REGISTER.md` — new TF-0045 entry
- `DOCS/Milestone_Roadmap_v2.md` — mark TF-0045 as Done

## Affected Entities

- `MarketDataProvider` Protocol — must be satisfied structurally (no inheritance)
- `MarketSnapshot`, `PriceOHLCV`, `ProviderProvenance` — output types (unchanged)
- `ProviderUnavailableError` — error boundary (unchanged)

## ADR Impact

ADR-0032 already anticipates all three M9 providers (yfinance, Polygon/Massive.com,
Alpaca) in its Consequences section. No new ADR is required.

## Event Implications

None. Provider adapters are read-only infrastructure. No events produced.

## Replay Implications

- `ProviderProvenance` carries `fetched_at` (retrieval time) and `data_as_of`
  (candle timestamp) for future replay-compatible persistence (TF-0052).
- Snapshots remain advisory, non-canonical.
- All tests are mocked — no real network calls, no external state dependency.

## Lifecycle Implications

None. Adapter has no interaction with lifecycle engine.

## Implementation Decisions

### API Key Handling
Polygon.io requires an API key. The key is accepted as a constructor parameter
(`api_key: str`). This is an infrastructure concern — domain and services layers
must never see the key or the SDK.

### Client Lifecycle
`RESTClient` is created in `__init__` for connection reuse. Tests mock `RESTClient`
before instantiation via `patch("src.infrastructure.market.polygon_adapter.RESTClient")`.

### Endpoint Choice
`client.get_previous_close_agg(symbol)` returns the previous day's OHLCV aggregate.
This mirrors what yfinance's `ticker.history(period="1d")` returns and is appropriate
for the advisory daily snapshot contract.

### Timestamp Conversion
Polygon timestamps are epoch milliseconds. Conversion:
`datetime.fromtimestamp(float(agg.timestamp) / 1000, tz=UTC)`

### Volume Normalization
Polygon may return volume as a float. Cast to int: `int(float(agg.volume))`.

### Provider Version
Use `importlib.metadata.version("polygon-api-client")` — more reliable than
accessing `polygon.__version__` since the SDK may not expose it at the top level.

### Error Handling
All SDK errors and empty-list responses are wrapped in `ProviderUnavailableError`.
Partial parse failures are caught and wrapped in the same error type.

### Symbol Normalization
Symbol is uppercased in `fetch_snapshot` before the API call, matching yfinance behavior.

## Testing Strategy

Mirror `tests/test_yfinance_adapter.py` structure:
- `TestPolygonProviderIdentity` — provider_id, provider_version properties
- `TestPolygonProviderFetchSuccess` — successful OHLCV mapping, advisory flag,
  provenance completeness, symbol casing, timestamp UTC normalization
- `TestPolygonProviderFetchFailure` — empty list, SDK exception, parse exception,
  ProviderUnavailableError subclass assertion
- `TestPolygonProviderFetchSnapshots` — tuple return, symbol order, all advisory,
  raises on unavailable symbol

All tests use `setup_method` to create provider with a mocked `RESTClient`.

## Scope Boundary

- In: adapter satisfying Protocol, full mocked test suite, dependency wiring
- Out: Alpaca adapter (TF-0046), workspace overlays (TF-0047), caching/rate-limit
  handling, intraday or multi-day history ranges, API key management beyond
  constructor parameter

## Unresolved Questions

None — pattern is well-established from TF-0044.
