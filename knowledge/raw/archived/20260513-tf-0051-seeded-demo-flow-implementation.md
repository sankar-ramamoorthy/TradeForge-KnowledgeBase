---
title: TF-0051 Seeded Demo Market Context Flow — Implementation Capture
type: raw-implementation-capture
issue: TF-0051
milestone: M9
status: raw
created: 2026-05-13
---

# TF-0051: Seeded Demo Market Context Flow — Implementation Capture

## What Was Built

### Infrastructure Layer: `src/infrastructure/market/seeded_provider.py`
- `SeededMarketDataProvider` — satisfies `MarketDataProvider` Protocol structurally
- Static `_DEMO_SEED` dict keyed by uppercase symbol → `_SeedEntry` NamedTuple
- Optional `fetched_at` injection for deterministic test timestamps
- `available_symbols` property returns sorted tuple of seeded symbols
- Raises `ProviderUnavailableError` for unknown symbols (consistent with live providers)
- Symbol input normalized to uppercase via `.upper().strip()`
- Provider ID: `"seeded-demo"`, version: `"1.0.0"`
- 7 demo symbols covering all 5 regime outcomes:
  - AAPL → BULL, TSLA → HIGH_VOLATILITY, NVDA → BULL
  - SPY → RANGING, QQQ → BEAR, GLD → RANGING, TLT → LOW_VOLATILITY

### ADR-0032 Compliance
No special-casing in domain/services/app layers. The seeded provider flows through the
exact same normalized boundary as yfinance/alpaca/polygon adapters. Demo mode is
achieved by injecting `SeededMarketDataProvider` into `MarketSnapshotService` —
no factory changes, no application-layer bypass.

## Test Coverage: 33 tests
- `TestSeededMarketDataProvider`: 10 tests (known/unknown symbols, provenance, invariants)
- `TestSeedDataRegimeClassification`: 10 tests (per-symbol regime + all-5-regimes coverage assertion)
- `TestSeededProvenanceTracking`: 4 tests (provenance recording with seeded provider)
- `TestDemoMarketContextAPI`: 9 tests (full M9 API pipeline via TestClient)

## Regime Coverage Verification
The test `test_all_five_regimes_covered_in_seed_data` explicitly asserts that all 5
interpretable regimes are present in the seed data. This acts as a guard: if the seed
data is changed or the interpreter thresholds shift, the test fails clearly.

## Integration with Existing Stack
- `SeededMarketDataProvider` + `SingleBarRegimeInterpreter` + `InMemoryProvenanceStore`
  compose into a complete demo session without modification to any other layer
- `create_app(market_snapshot_service=..., provenance_query_service=...)` injection
  already provided the necessary hook (no `create_app` changes required)

## Verification
- `uv run pytest tests/test_seeded_demo_flow.py` — 33 passed
- `uv run pytest` — 448 passed
- `uv run ruff check .` — clean
- `uv run mypy src tests` — clean (92 files)

## Operational Lessons
- Seeded data for deterministic tests requires careful OHLCV invariant checking
  (low ≤ open ≤ high, low ≤ close ≤ high) — PriceOHLCV validates at construction
- TLT LOW_VOLATILITY regime needed a very tight range: (92.20-91.85)/92 = 0.0038 < 0.005
- QQQ BEAR needed a clear directional move: (433-446)/446 = -2.9% < -2% threshold
- The `_SeedEntry` NamedTuple carries inline regime comments for documentation clarity
