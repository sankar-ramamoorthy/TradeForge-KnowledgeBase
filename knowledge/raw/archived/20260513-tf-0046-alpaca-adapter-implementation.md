---
title: TF-0046 Implementation — Alpaca Market Data Adapter
type: implementation-capture
status: raw
issue: TF-0046
milestone: M9
created: 2026-05-13
---

# TF-0046 Implementation: Add Alpaca Market Data Adapter

## Summary

Implemented `AlpacaProvider` satisfying the `MarketDataProvider` Protocol
structurally. Adapter uses `alpaca-py>=0.30` (`alpaca-py==0.43.4` installed).
SDK coupling is fully contained in `src/infrastructure/market/alpaca_adapter.py`.
Domain and services layers have no alpaca imports.

## Files Changed

| File | Action |
|---|---|
| `src/infrastructure/market/alpaca_adapter.py` | New — AlpacaProvider adapter |
| `tests/test_alpaca_adapter.py` | New — 25 mocked tests |
| `pyproject.toml` | Added `alpaca-py>=0.30` dependency + mypy override |

## Key Implementation Notes

### API Endpoint
`StockHistoricalDataClient.get_stock_bars(StockBarsRequest(...))` with
`timeframe=TimeFrame.Day` and `start = now - 5 days`. Takes `bars[-1]`
(most recent bar in ascending-time response). The 5-day lookback ensures
recent data is available across weekends and holidays.

### Two-Param Constructor
Alpaca authentication requires both `api_key` and `secret_key`. Both are
accepted as constructor params, infrastructure concern only.

### Response Access Pattern
`response[upper_symbol]` with explicit `KeyError` handling (symbol absent → bars = []).
`BarSet` is dict-like in alpaca-py; plain dict mock works in tests.

### Timestamp Normalization
`Bar.timestamp` is already a `datetime` object — simpler than Polygon's epoch ms.
Normalized to UTC: naive → `replace(tzinfo=UTC)`; aware → `astimezone(UTC)`.

### Volume
Cast: `int(float(bar.volume))` — volume arrives as float from Alpaca.

### Provider Version
`importlib.metadata.version("alpaca-py")` — returned `0.43.4`.

### mypy Configuration
Added `[[tool.mypy.overrides]]` for `alpaca` and `alpaca.*` with
`ignore_missing_imports = true`. No unused-ignore issues (unlike the
polygon adapter where a redundant inline comment triggered mypy).

## Tests Executed

- `uv run pytest tests/test_alpaca_adapter.py` — 25 passed (all mocked)
- `uv run pytest` — 322 passed (1 websockets deprecation warning from alpaca-py, benign)
- `uv run ruff check .` — clean
- `uv run mypy src tests` — clean (80 files)

## Deprecation Warning Note

`alpaca-py==0.43.4` emits one `DeprecationWarning` about `websockets.legacy` during
import. This is an internal alpaca-py dependency warning, not a TradeForge issue.
The warning does not affect adapter behavior and does not appear during normal
runtime (only in pytest's warning collection). No action required.

## M9 Provider Adapter Pattern: Complete

All three M9 provider adapters are now implemented:

| Issue | Provider | SDK | Constructor | Tests |
|---|---|---|---|---|
| TF-0044 | yfinance | `yfinance>=1.3.0` | no params | 20 passed |
| TF-0045 | Polygon.io | `polygon-api-client>=1.0` | `api_key` | 23 passed |
| TF-0046 | Alpaca | `alpaca-py>=0.30` | `api_key + secret_key` | 25 passed |

The `MarketDataProvider` Protocol boundary (TF-0042) successfully absorbed three
different vendor SDK shapes without any changes to domain or services layers.
This confirms the Protocol port pattern is resilient to provider heterogeneity.

## Follow-up Opportunities

- TF-0047: Market context workspace overlays (first consumer of all three providers)
- TF-0048: Market regime interpretation model
- TF-0052: Replay-compatible market snapshot persistence
