---
title: Planning — TF-0045 Polygon/Massive.com Provider Adapter
type: processed-knowledge
status: processed
source: knowledge/raw/20260513-tf-0045-polygon-adapter-planning.md
issue: TF-0045
milestone: M9
created: 2026-05-13
---

# TF-0045 Planning: Polygon.io / Massive.com Provider Adapter

## Key Planning Decisions

### Endpoint: Previous-Close Aggregate
`get_previous_close_agg(symbol)` returns the most recent completed daily OHLCV.
This mirrors yfinance `history(period="1d")` behavior and is appropriate for
the advisory daily snapshot contract.

### API Key Pattern
API key accepted as constructor parameter (`api_key: str`). Infrastructure
concern only — not exposed to domain or services layers. `RESTClient` created
once in `__init__` for connection reuse.

### Version Detection
`importlib.metadata.version("polygon-api-client")` preferred over
`polygon.__version__` — more reliable across SDK versions.

### Test Strategy
Mock `polygon.RESTClient` via `patch("src.infrastructure.market.polygon_adapter.RESTClient")`.
Provider instantiated with mocked client in `setup_method`. All 23 tests are
fully mocked — no real network calls.

### ADR Impact
None required. ADR-0032 already covers all M9 providers in its Consequences section.

## Architectural Confirmation

The `MarketDataProvider` Protocol (TF-0042) was designed to be satisfied structurally
without inheritance. This held true for both yfinance (TF-0044) and Polygon (TF-0045),
confirming the Protocol boundary is portable and replaceable as intended.
