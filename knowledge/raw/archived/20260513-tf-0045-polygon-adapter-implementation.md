---
title: TF-0045 Implementation — Polygon/Massive.com Provider Adapter
type: implementation-capture
status: raw
issue: TF-0045
milestone: M9
created: 2026-05-13
---

# TF-0045 Implementation: Add Massive.com (Polygon.io) Market Data Adapter

## Summary

Implemented `PolygonProvider` satisfying the `MarketDataProvider` Protocol
structurally. Adapter uses `polygon-api-client>=1.0` (`polygon-api-client==1.16.3`
installed). SDK coupling is fully contained in
`src/infrastructure/market/polygon_adapter.py`. Domain and services layers have
no polygon imports.

## Files Changed

| File | Action |
|---|---|
| `src/infrastructure/market/polygon_adapter.py` | New — PolygonProvider adapter |
| `tests/test_polygon_adapter.py` | New — 23 mocked tests |
| `pyproject.toml` | Added `polygon-api-client>=1.0` dependency + mypy override |

## Key Implementation Notes

### API Endpoint
Used `client.get_previous_close_agg(symbol)` — Polygon's previous-close aggregate
endpoint returns the most recent completed daily OHLCV candle. Returns a list;
adapter takes `aggs[0]` (most recent).

### Timestamp Conversion
Polygon aggregate timestamps are epoch milliseconds. Conversion:
`datetime.fromtimestamp(float(agg.timestamp) / 1000, tz=UTC)`.

### Volume
Polygon returns volume as float from the API. Cast to `int(float(agg.volume))`
for normalized contract consistency.

### Provider Version
Used `importlib.metadata.version("polygon-api-client")` via `_sdk_version()`
helper function. More reliable than `polygon.__version__` which isn't guaranteed
to exist in all SDK versions.

### Client Lifecycle
`RESTClient` created once in `__init__(self, api_key: str)` for connection reuse.
API key is an infrastructure concern only — not exposed to domain/services.

### mypy Configuration
Added `[[tool.mypy.overrides]]` for `polygon` and `polygon.*` with
`ignore_missing_imports = true`. Ruff's isort merged the `polygon` import into
the same block as `src.*` imports (expected behavior given the project's
`src = ["src", "tests"]` ruff config).

## Tests Executed

- `uv run pytest tests/test_polygon_adapter.py` — 23 passed (all mocked)
- `uv run pytest` — 297 passed
- `uv run ruff check .` — clean
- `uv run mypy src tests` — clean (78 files)

## Replay Implications

`ProviderProvenance` on all snapshots carries `fetched_at` (UTC time of retrieval)
and `data_as_of` (UTC timestamp of the candle). This satisfies the replay-integrity
requirement from ADR-0032 that enables TF-0052 (replay-compatible snapshot persistence).

## Architectural Observations

The three-provider M9 pattern (yfinance → Polygon → Alpaca) is consistent and
well-established. Each adapter:
1. Satisfies Protocol structurally (no inheritance)
2. Contains all SDK coupling in a single file
3. Maps vendor-specific timestamp/price formats to the normalized `PriceOHLCV`
4. Wraps all SDK errors in `ProviderUnavailableError`
5. Tests mock the SDK entry point entirely

This pattern is stable and can be replicated for TF-0046 (Alpaca) with minimal variation.

## Follow-up Opportunities

- TF-0046: Alpaca adapter (next in M9 provider sequence)
- TF-0047: Market context workspace overlays (consumes all providers)
- TF-0052: Replay-compatible market snapshot persistence
