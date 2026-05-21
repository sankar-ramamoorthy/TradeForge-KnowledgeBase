---
title: TF-0044 yfinance Provider Adapter — Planning Capture
type: raw-planning
status: raw
created: 2026-05-13
issue: TF-0044
milestone: M9
---

# TF-0044: Add Read-Only yfinance Provider Adapter — Planning Capture

## Issue Goal

Implement the first concrete MarketDataProvider adapter backed by yfinance.
This is a read-only infrastructure adapter — it satisfies the Protocol from TF-0042,
returns advisory MarketSnapshot objects, and wraps all SDK failures in ProviderUnavailableError.

## Dominant Task Category

Infrastructure integration / external API adapter.

Context bundle: INVARIANTS + ADR-0032 + provider boundary model (TF-0042) + snapshot service (TF-0043).

## Architecture Layer Affected

- `src/infrastructure/market/` — new infrastructure module
  - `yfinance_adapter.py` — YFinanceProvider class
  - `__init__.py`
- `pyproject.toml` — add yfinance runtime dependency
- `pyproject.toml` — add mypy overrides for yfinance and pandas (no stubs)

No domain or services changes.

## Key Design Decisions

### 1. Infrastructure owns all yfinance SDK coupling

The yfinance import lives only in `src/infrastructure/market/yfinance_adapter.py`.
Domain and services layers never import yfinance. This enforces ADR-0032 replaceability.

### 2. ticker.history(period="1d") for latest OHLCV

Uses the most recent row from the 1-day history DataFrame.
Handles timezone-aware DatetimeIndex — as_of carries UTC.

### 3. Decimal via str(float) for price precision

yfinance returns numpy float64 values. Converting via str() then Decimal()
avoids binary float → Decimal precision surprises.

### 4. fetch_snapshots iterates fetch_snapshot per symbol

Simpler than parsing batch yfinance.download() output for MVP.
Batch optimization is a future concern, not M9.

### 5. provider_version reads yf.__version__ dynamically

Adapter carries the installed yfinance library version in provenance,
so replay snapshots record exactly which library version produced the data.

### 6. Wrap ALL SDK exceptions in ProviderUnavailableError

Two failure modes:
- Empty DataFrame (symbol not found / no data)
- Any exception from yfinance (network, parsing, rate limit)

Both become ProviderUnavailableError so callers have a single explicit type to handle.

### 7. Tests use mocks — no real network calls

Tests mock the yfinance.Ticker object. Real network calls would make tests
slow, flaky, and dependent on market hours.

### 8. mypy overrides for yfinance and pandas

Neither package ships complete type stubs. Add overrides in pyproject.toml:
  module = ["yfinance", "yfinance.*", "pandas", "pandas.*"]
  ignore_missing_imports = true

## Entities Introduced

- `YFinanceProvider` (class in infrastructure/market) — satisfies MarketDataProvider Protocol

## Replay Implication

provenance.data_as_of = hist.index[-1] (the timestamp of the candle from yfinance).
This is the field TF-0052 will use for historical reconstruction.

## Lifecycle Implication

None. Read-only.

## Event Model Implication

None. No event ledger writes.

## Testing Strategy

- Mock yfinance.Ticker.history() to return a realistic pandas DataFrame
- Test correct OHLCV mapping (Decimal values, volume as int)
- Test provider_id == "yfinance"
- Test provenance.provider_id matches
- Test ProviderUnavailableError on empty DataFrame
- Test ProviderUnavailableError on SDK exception
- Test is_advisory=True on result
- Test fetch_snapshots returns ordered results

## Out of Scope

- Polygon/Massive.com adapter (TF-0045)
- Alpaca adapter (TF-0046)
- Caching / rate limiting
- Intraday or multi-day history ranges
- Batch yfinance.download() optimization
