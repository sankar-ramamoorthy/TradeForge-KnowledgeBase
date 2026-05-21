---
title: TF-0044 yfinance Provider Adapter — Implementation Capture
type: raw-implementation
status: raw
created: 2026-05-13
issue: TF-0044
milestone: M9
---

# TF-0044: yfinance Provider Adapter — Implementation Capture

## Implementation Summary

Created `src/infrastructure/market/` with two files:
- `yfinance_adapter.py` — YFinanceProvider class
- `__init__.py`

Added yfinance>=1.3.0 to runtime dependencies in pyproject.toml.
Added mypy overrides for yfinance and pandas (ignore_missing_imports=true).
Inline `# type: ignore` on the import line was NOT needed — the pyproject.toml
override made it redundant (mypy flagged it as unused-ignore).

One test file:
- `tests/test_yfinance_adapter.py` — 20 tests, all mocked, no real network calls

## Key Implementation Details

### _parse_latest_ohlcv helper function

Extracted to a private module-level function for clarity and testability.
Takes the symbol string and hist DataFrame, returns PriceOHLCV.

Price conversion: `Decimal(str(float(row["Open"])))` — converts numpy float64
via float() for Python numeric, then str() for decimal-safe parsing.
Direct `Decimal(float(...))` produces binary float representation artifacts.

Volume: `int(row["Volume"])` — handles numpy int64 cleanly.

Timezone: `ts.to_pydatetime()` returns a Python datetime; if tz-naive, we
replace with UTC. If tz-aware, we convert to UTC with `.astimezone(UTC)`.

### Error handling

Two distinct failure points wrapped in ProviderUnavailableError:
1. At `yf.Ticker(symbol)` / `ticker.history(period="1d")` call — SDK errors
2. At `hist.empty` check — symbol not found / no data
3. At `_parse_latest_ohlcv` call — malformed response data

All three map to ProviderUnavailableError so callers have a single type.

### provider_version reads yf.__version__ dynamically

If yfinance removes __version__ in a future release, falls back to "unknown"
rather than crashing — defensive but explicit.

### fetch_snapshots iterates per-symbol

Simple loop over fetch_snapshot. No batch yf.download() optimization.
Keeps failure semantics consistent — raises on first unavailable symbol.

## mypy override approach

Rather than inline `# type: ignore[import-untyped]` on the import line,
a `[[tool.mypy.overrides]]` section in pyproject.toml covers both yfinance
and pandas at the module level. This is cleaner and handles all yfinance
sub-imports without per-line annotations.

## Test coverage

20 tests across:
- Identity: provider_id="yfinance", provider_version non-empty string
- Success: symbol uppercasing, OHLCV Decimal mapping, prices are Decimal,
  as_of matches index, naive tz normalized, provenance fields,
  is_advisory=True, latest row used from multi-row DataFrame
- Failure: empty DataFrame → ProviderUnavailableError, SDK exception →
  ProviderUnavailableError, history() exception → ProviderUnavailableError,
  ProviderUnavailableError is Exception subclass
- fetch_snapshots: returns tuple, symbol order preserved, all advisory,
  raises on unavailable symbol

## Verification Commands

- `uv run pytest tests/test_yfinance_adapter.py` — 20 passed (all mocked)
- `uv run pytest` — 274 passed
- `uv run ruff check .` — clean
- `uv run mypy src tests` — clean (76 files)

## Follow-up Issues

- TF-0045: Polygon/Massive.com adapter (same Protocol, different SDK)
- TF-0046: Alpaca adapter (same Protocol, different SDK)
- TF-0051: Seeded demo flow (uses the normalized boundary, not real provider)
