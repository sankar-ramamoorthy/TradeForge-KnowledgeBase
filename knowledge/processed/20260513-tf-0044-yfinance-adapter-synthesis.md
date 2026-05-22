---
title: TF-0044 Synthesis — yfinance Provider Adapter
type: processed-synthesis
status: processed
issue: TF-0044
milestone: M9
created: 2026-05-13
source_history:
  - knowledge/raw/20260513-tf-0044-yfinance-adapter-planning.md
  - knowledge/raw/20260513-tf-0044-yfinance-adapter-implementation.md
tags: [market-data, provider-adapter, infrastructure, yfinance, M9]
related:
  - "[[Provider Boundary Model]]"
---

# TF-0044 Synthesis — yfinance Provider Adapter

## Stabilized Understanding

`YFinanceProvider` is a read-only infrastructure adapter that satisfies the `MarketDataProvider` Protocol structurally. It is the first concrete provider implementation, backed by the yfinance library, and sets the pattern for subsequent adapters (Polygon, Alpaca).

## Key Design Decisions Preserved

- yfinance SDK coupling is fully isolated in `src/infrastructure/market/yfinance_adapter.py`. Domain and services layers never import yfinance.
- Decimal conversion uses `Decimal(str(float(row["Open"])))` — avoids binary float → Decimal precision artifacts from numpy float64.
- All SDK failures (empty DataFrame, network errors, parsing errors) map to a single `ProviderUnavailableError` type so callers have one explicit type to handle.
- `provider_version` reads `yf.__version__` dynamically — provenance records carry the exact library version that produced the data, supporting replay integrity.
- Tests use mocked `yf.Ticker` — no real network calls, no market-hours dependency.
- mypy overrides for yfinance and pandas are in `pyproject.toml` (module-level), not inline `# type: ignore`.

## Boundary Rule

This adapter introduces no lifecycle events, no event ledger writes, and no domain state. It is infrastructure only.

## Reusable Pattern

Every provider adapter (Polygon, Alpaca, SeededDemo) follows the same structure: satisfy `MarketDataProvider` Protocol structurally, wrap all SDK failures as `ProviderUnavailableError`, record `provider_id` and `data_as_of` in provenance.
