---
title: Planning — TF-0046 Alpaca Market Data Adapter
type: processed-knowledge
status: processed
source: knowledge/raw/20260513-tf-0046-alpaca-adapter-planning.md
issue: TF-0046
milestone: M9
created: 2026-05-13
---

# TF-0046 Planning: Alpaca Market Data Adapter

## Key Planning Decisions

### Two Constructor Params
Alpaca requires both `api_key` and `secret_key`. Both are infrastructure concerns
only — not exposed to domain or services layers.

### Endpoint: Stock Bars with Lookback
`get_stock_bars(StockBarsRequest(timeframe=TimeFrame.Day, start=now-5days))`
then take `bars[-1]`. Using a 5-day lookback rather than `limit=1` avoids
ambiguity about ascending/descending sort order in the Alpaca API.

### Timestamp Already a Datetime
Unlike Polygon (epoch milliseconds), Alpaca `Bar.timestamp` is already a
`datetime` object. Normalize via tzinfo check rather than millisecond conversion.

### Response Pattern
`response[symbol]` with explicit `KeyError` → `bars = []` → `ProviderUnavailableError`.
Plain dict mock works in tests since `BarSet` supports dict-style access.

### Test Mock Strategy
Mock `StockHistoricalDataClient`. Response mocked as plain dict `{"SYMBOL": [bars]}`.
`StockBarsRequest` + `TimeFrame` are real objects constructed inside `fetch_snapshot`
and passed to the mocked client — no need to mock them.

## Architectural Confirmation

The `MarketDataProvider` Protocol absorbed three significantly different vendor SDKs
(yfinance, Polygon, Alpaca) without any changes to domain or services layers.
This validates the Protocol port pattern as vendor-agnostic and resilient to
SDK heterogeneity.
