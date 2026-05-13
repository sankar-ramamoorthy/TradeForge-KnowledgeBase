---
title: Implementation — TF-0045 Polygon/Massive.com Provider Adapter
type: processed-knowledge
status: processed
source: knowledge/raw/20260513-tf-0045-polygon-adapter-implementation.md
issue: TF-0045
milestone: M9
created: 2026-05-13
---

# TF-0045 Implementation: Polygon.io / Massive.com Provider Adapter

## Outcome

`PolygonProvider` implemented and verified. Satisfies `MarketDataProvider` Protocol
structurally. 23 mocked tests, all passing. Full suite 297 passed. ruff and mypy clean.

## Stable Provider Adapter Pattern

Three-adapter M9 sequence (yfinance TF-0044, Polygon TF-0045, Alpaca TF-0046) has
established a stable, replicable pattern:

1. Protocol satisfied structurally — no inheritance
2. SDK coupling fully contained in single infrastructure file
3. All vendor-specific types (timestamps, price precision, volume type) normalized at
   the adapter boundary
4. All SDK errors wrapped in `ProviderUnavailableError`
5. `RESTClient` / equivalent created in `__init__` for connection reuse
6. Tests mock the SDK entry point; `setup_method` creates provider with mocked client
7. `mypy.overrides` with `ignore_missing_imports = true` for each new SDK

## Polygon-Specific Normalizations

| Polygon API behavior | Normalization |
|---|---|
| Timestamps in epoch milliseconds | `datetime.fromtimestamp(ms / 1000, tz=UTC)` |
| Volume as `float` | `int(float(agg.volume))` |
| `get_previous_close_agg` returns list | take `aggs[0]` (most recent) |

## Replay Integrity

`ProviderProvenance.fetched_at` = UTC time of API call.
`ProviderProvenance.data_as_of` = UTC timestamp of the returned candle.
Both required for TF-0052 (replay-compatible snapshot persistence).

## ruff isort Behavior

With `src = ["src", "tests"]` in ruff config and no explicit `known-third-party`
settings, ruff's isort merges the `polygon` (third-party) import into the same
block as `src.*` (first-party) imports. This is expected behavior and does not
affect runtime behavior.

## Next: TF-0046

Alpaca adapter follows the same structural pattern. Key differences expected:
- Alpaca uses a different Python SDK (`alpaca-py` or `alpaca-trade-api`)
- Alpaca's historical data endpoint structure differs
- Alpaca requires API key + secret (two constructor params vs one)
