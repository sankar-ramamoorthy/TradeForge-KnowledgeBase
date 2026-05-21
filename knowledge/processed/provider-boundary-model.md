---
title: Provider Boundary Model
type: processed-knowledge
status: processed
created: 2026-05-13
source_issues: [TF-0042]
source_history:
  - knowledge/raw/20260513-tf-0042-provider-boundary-interfaces-planning.md
  - knowledge/raw/20260513-tf-0042-provider-boundary-interfaces-implementation.md
milestone: M9
related_adrs: [ADR-0010, ADR-0032]
tags: [market-context, provider-boundary, advisory, replayability, provenance]
---

# Provider Boundary Model

## Core Concept

All external market data providers connect to TradeForge through a normalized
boundary that enforces read-only, advisory, non-canonical data flow.

This boundary is architecturally prior to all provider adapter implementations.

## Key Entities

| Entity | Layer | Purpose |
|---|---|---|
| `MarketRegime` | domain | Advisory regime classification (never canonical) |
| `ProviderProvenance` | domain | Origin record for replay integrity |
| `PriceOHLCV` | domain | Normalized OHLCV price record |
| `MarketSnapshot` | domain | Complete advisory snapshot (price + provenance + regime) |
| `MarketDataProvider` | domain (Protocol) | Provider port interface |
| `ProviderUnavailableError` | domain | Explicit failure type |

## Critical Invariant: fetched_at vs data_as_of

`ProviderProvenance` carries two timestamps deliberately:

- `fetched_at` — when the data was retrieved from the provider
- `data_as_of` — what market moment the data represents

This distinction is required for historical reconstruction:
a decision made on 2026-05-13 may have been informed by a snapshot
whose `data_as_of` is 2026-05-12 16:00 UTC (prior day's close).
Without this distinction, replay cannot determine what market context
was visible at the decision point.

## Advisory Boundary Contract

`MarketSnapshot.is_advisory` always returns `True`.

This is a machine-readable contract. Any consumer must be able to confirm
the advisory boundary before use in workspace or projection logic.

## Replaceability Principle

The `MarketDataProvider` Protocol uses structural subtyping.

Provider adapters (yfinance, Polygon, Alpaca) do not inherit from a base class.
They satisfy the Protocol structurally. This means swapping providers
requires only a new adapter file — no changes to service or workspace layers.

## What This Boundary Prevents

- Provider schema changes propagating into workspace projections
- External data becoming canonical event ledger entries
- Provider-specific SDK coupling bleeding across architectural layers
- Silent empty/stale context on provider failure (ProviderUnavailableError enforces explicit handling)

## Downstream Activation

- TF-0043: services-layer snapshot normalization
- TF-0044 to TF-0046: concrete provider adapters implementing this Protocol
- TF-0047: workspace context overlays consuming normalized snapshots
- TF-0052: replay-compatible snapshot persistence using `data_as_of` as the historical anchor
