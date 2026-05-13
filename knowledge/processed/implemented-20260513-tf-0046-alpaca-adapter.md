---
title: Implementation — TF-0046 Alpaca Market Data Adapter
type: processed-knowledge
status: processed
source: knowledge/raw/20260513-tf-0046-alpaca-adapter-implementation.md
issue: TF-0046
milestone: M9
created: 2026-05-13
---

# TF-0046 Implementation: Alpaca Market Data Adapter

## Outcome

`AlpacaProvider` implemented and verified. 25 mocked tests passing. Full suite
322 passed. ruff and mypy clean (80 files).

## M9 Provider Adapter Pattern: Now Complete

All three M9 adapters share a stable, canonical implementation pattern:

| Issue | Provider | SDK | Constructor params |
|---|---|---|---|
| TF-0044 | yfinance | `yfinance>=1.3.0` | none |
| TF-0045 | Polygon.io / Massive.com | `polygon-api-client>=1.0` | `api_key` |
| TF-0046 | Alpaca | `alpaca-py>=0.30` | `api_key + secret_key` |

## Alpaca-Specific Normalizations

| Alpaca API behavior | Normalization |
|---|---|
| `Bar.timestamp` is a `datetime` object | check `tzinfo`; attach UTC if naive |
| Volume as `float` | `int(float(bar.volume))` |
| Response: `BarSet` (dict-like) | `response[symbol]` with `KeyError` guard |
| Returns ascending time order | take `bars[-1]` for most recent |

## Protocol Resilience Confirmed

`MarketDataProvider` Protocol (TF-0042) successfully absorbed three vendor SDKs
with no changes to domain or services layers:
- yfinance: pandas DataFrame → epoch-ms timestamp
- Polygon: `get_previous_close_agg` → epoch-ms timestamp
- Alpaca: `StockBarsRequest` → datetime object already

This confirms the normalized provider boundary pattern is architecturally robust.

## Benign Warning Note

`alpaca-py==0.43.4` emits a `DeprecationWarning` about `websockets.legacy` during
import (internal SDK issue). No action required.

## Next Steps in M9

- TF-0047: Market context workspace overlays (first consumer of all three providers)
- TF-0048: Market regime interpretation model
- TF-0049: Contextual operational summaries
- TF-0050: Provider provenance tracking
