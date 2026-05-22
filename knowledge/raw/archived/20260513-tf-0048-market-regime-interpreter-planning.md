---
title: TF-0048 Planning — Market Regime Interpretation Model
type: planning-capture
status: raw
issue: TF-0048
milestone: M9
created: 2026-05-13
---

# TF-0048 Planning: Market Regime Interpretation Model

## Issue Goal

Assign a deterministic, auditable market regime classification to advisory
MarketSnapshots. The MarketRegime enum (bull/bear/ranging/high-volatility/
low-volatility/unknown) already exists in domain/market/snapshot.py.
MarketSnapshot.regime defaults to UNKNOWN. This issue adds the interpreter
logic that populates the regime field.

## Affected Layers

- domain: MarketRegimeInterpreter Protocol (src/domain/market/regime.py)
- services: SingleBarRegimeInterpreter (src/services/market/regime_interpreter.py)
- services: MarketSnapshotService receives optional regime_interpreter param
- app: wire SingleBarRegimeInterpreter into create_app() default
- frontend: display regime badge in MarketContextPanel
- tests: test_market_regime_interpreter.py

## Key Design Decisions

### Protocol in Domain, Implementation in Services
Follows MarketDataProvider pattern: Protocol boundary in domain defines the
contract; services layer contains the deterministic implementation. Allows future
alternative interpreters (Bollinger-band-based, AI-based in M10) without coupling.

### Single-Bar Rules (deterministic, explicit)
A single daily OHLCV bar supports limited but deterministic regime signals.
Rules applied in priority order:

1. HIGH_VOLATILITY: (high - low) / open > 4%  (large intraday range)
2. LOW_VOLATILITY:  (high - low) / open < 0.5% (compressed range)
3. BULL:            (close - open) / open > 2%  (strong bullish close)
4. BEAR:            (close - open) / open < -2% (strong bearish close)
5. RANGING:         all other cases

UNKNOWN: fallback on any calculation error or zero open.

These classify "today's bar character", not multi-week market regime.
Docstring is explicit about this limitation. Multi-bar regime requires
historical data fetching — future scope.

### Regime Annotation in MarketSnapshotService
Add optional regime_interpreter: MarketRegimeInterpreter | None = None param.
When set, both fetch_context and fetch_snapshot annotate snapshots using
dataclasses.replace(snapshot, regime=interpreter.interpret(snapshot)).
No changes to existing test assertions (regime was UNKNOWN, stays UNKNOWN
if no interpreter provided).

### API and Frontend
No new API endpoints. regime field already exists in MarketSnapshotOverlayResponse
and is already serialized (as "unknown"). With interpreter wired, it will now
return meaningful values. Frontend MarketContextPanel adds a regime badge to
SnapshotRow for visibility.

### ADR Impact
None. ADR-0032 covers the provider boundary. Regime interpretation is a
services-layer rule engine — consistent with INVARIANTS.md "Deterministic Rule
Evaluation" and "Derived State Must Remain Distinguishable".

## Scope Boundary

- In: Protocol, SingleBarRegimeInterpreter, snapshot_service annotation,
  application wiring, frontend regime badge, tests
- Out: multi-bar historical regime (needs historical data fetching),
  AI-based regime interpretation (TF-0053+), regime storage/persistence
