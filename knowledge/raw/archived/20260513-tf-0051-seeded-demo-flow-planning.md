---
title: TF-0051 Seeded Demo Market Context Flow — Planning Capture
type: raw-planning-capture
issue: TF-0051
milestone: M9
status: raw
created: 2026-05-13
---

# TF-0051: Seeded Demo Market Context Flow — Planning Capture

## Goal

Add a deterministic seeded market data provider that enables demonstration of
the full M9 market context stack without live API keys or network access.

## ADR-0032 Constraint

"The demo seed flow (TF-0051) must use the same normalized boundary as live
providers to prevent demo/production divergence."

This means: `SeededMarketDataProvider` must implement `MarketDataProvider`
Protocol structurally. No special-casing anywhere in domain/services/app layers.

## Design

### Infrastructure: `SeededMarketDataProvider`
- Satisfies `MarketDataProvider` Protocol (same as yfinance/alpaca/polygon adapters)
- Holds static OHLCV seed data for a curated set of demo symbols
- Returns deterministic `MarketSnapshot` with full `ProviderProvenance`
- Raises `ProviderUnavailableError` for unknown symbols (consistent with live providers)
- Optional `fetched_at` injection for deterministic test timestamps
- `available_symbols` property exposes the demo symbol set

### Seed Data Design
Cover all 5 regime outcomes to demonstrate the full interpretation model:
- AAPL: BULL (close > open by >2%, range moderate)
- TSLA: HIGH_VOLATILITY (range > 4%)
- NVDA: BULL (different sector, confirms regime applies across symbols)
- SPY: RANGING (broad market neutral day)
- QQQ: BEAR (tech ETF, down day)
- GLD: RANGING (gold neutral)
- TLT: LOW_VOLATILITY (bond ETF, tight range)

### Demo Flow Integration Test
End-to-end test exercising the complete M9 stack with the seeded provider:
1. SeededMarketDataProvider → MarketSnapshotService → regime annotation
2. Provenance recording → ProvenanceQueryService query
3. ContextualSummaryService with seeded market context
4. API: GET /workspaces/market-context → correct advisory overlay per symbol
5. API: GET /workspaces/contextual-summary → combined summary with market notes
6. API: GET /provenance/market-data → fetch records for demo session

## ADR Impact

None — ADR-0032 already establishes the normalized boundary requirement.

## No Changes To `create_app()`

The existing `create_app(market_snapshot_service=...)` injection already
supports seeded provider injection. No new factory function needed.

## Invariants

- Layer Separation: SeededMarketDataProvider is infrastructure, not domain
- Market Intelligence Is Interpreted Context: seed data is still advisory
- Event Integrity: no event ledger writes from seeded provider
- Replay: seeded provider is deterministic — demo can be replayed exactly

## Testing Strategy

- Unit tests: SeededMarketDataProvider behavior (known/unknown symbols, provenance)
- Integration tests: full M9 pipeline with seeded provider via TestClient
- Verify regime classifications match expected values for seed data
- Verify provenance records created per fetch
