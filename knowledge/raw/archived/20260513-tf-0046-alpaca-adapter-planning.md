---
title: TF-0046 Planning — Alpaca Market Data Adapter
type: planning-capture
status: raw
issue: TF-0046
milestone: M9
created: 2026-05-13
---

# TF-0046 Planning: Add Alpaca Market Data Adapter

## Issue Goal

Add a read-only Alpaca provider adapter that satisfies the `MarketDataProvider`
Protocol defined in TF-0042. This completes the third and final planned M9 provider
adapter (yfinance TF-0044, Polygon TF-0045, Alpaca TF-0046).

## Affected Layer

infrastructure (`src/infrastructure/market/alpaca_adapter.py`)

## Affected Modules

- `src/infrastructure/market/alpaca_adapter.py` — new adapter class
- `tests/test_alpaca_adapter.py` — new test suite (~21 tests, all mocked)
- `pyproject.toml` — add `alpaca-py>=0.30` dependency + mypy override
- `DOCS/ISSUE_REGISTER.md` — new TF-0046 entry
- `DOCS/Milestone_Roadmap_v2.md` — mark TF-0046 as Done

## Key Differences from Polygon Adapter

| Aspect | Polygon | Alpaca |
|---|---|---|
| Constructor params | `api_key` (1) | `api_key` + `secret_key` (2) |
| SDK package | `polygon-api-client` | `alpaca-py` |
| Client class | `RESTClient` | `StockHistoricalDataClient` |
| Request class | N/A (method call) | `StockBarsRequest` |
| Timestamp format | epoch milliseconds | `datetime` object (UTC-aware) |
| Volume type | float | float |
| Response structure | `list(client.get_previous_close_agg(symbol))` | `response[symbol]` -> list of Bar |

## Implementation Decisions

### Two Constructor Params
Alpaca authentication requires both API key and secret. Accept both as constructor
params. Infrastructure concern only.

### Endpoint Choice
`StockHistoricalDataClient.get_stock_bars(StockBarsRequest(...))` with:
- `timeframe=TimeFrame.Day`
- `start = now - 5 days` (ensures recent bars; handles weekends/holidays)
- Take `bars[-1]` as the most recent available bar

Using a 5-day lookback rather than `limit=1` avoids ambiguity about whether
Alpaca returns oldest or newest bar when limit is specified.

### Timestamp Normalization
Alpaca `Bar.timestamp` is already a `datetime` object. May be UTC-aware or naive.
Normalize: if `tzinfo` is None → attach UTC; else → `astimezone(UTC)`.

### Volume
Alpaca volume may arrive as float. Cast: `int(float(bar.volume))`.

### Response Access Pattern
```python
response = self._client.get_stock_bars(request)
bars = list(response[upper_symbol])
```
Handle `KeyError` explicitly (symbol absent from response → empty bars → ProviderUnavailableError).

### Provider Version
`importlib.metadata.version("alpaca-py")`

### Test Strategy
Mock `StockHistoricalDataClient` via `patch`. Response mocked as dict: `{"AAPL": [mock_bar]}`.
`StockBarsRequest` is a real dataclass (not mocked) — constructed normally inside
`fetch_snapshot`, passed to the mocked client method.

## ADR Impact

None. ADR-0032 already covers all M9 providers.

## Scope Boundary

- In: adapter satisfying Protocol, full mocked test suite, dependency wiring
- Out: workspace overlays (TF-0047), snapshot persistence (TF-0052), caching,
  rate-limit handling, intraday history, Alpaca broker execution (different SDK)
