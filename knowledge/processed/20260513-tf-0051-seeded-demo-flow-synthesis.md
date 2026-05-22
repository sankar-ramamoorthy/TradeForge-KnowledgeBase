---
title: TF-0051 Synthesis — Seeded Demo Market Context Flow
type: processed-synthesis
status: processed
issue: TF-0051
milestone: M9
created: 2026-05-13
source_history:
  - knowledge/raw/20260513-tf-0051-seeded-demo-flow-planning.md
  - knowledge/raw/20260513-tf-0051-seeded-demo-flow-implementation.md
tags: [demo, seeded-data, provider-adapter, advisory, M9, demoability]
related:
  - "[[Provider Boundary Model]]"
  - "[[Demo Scenario Architecture]]"
---

# TF-0051 Synthesis — Seeded Demo Market Context Flow

## Stabilized Understanding

`SeededMarketDataProvider` is a deterministic infrastructure adapter that enables full M9 stack demonstration without live API keys or network access. It satisfies the same `MarketDataProvider` Protocol as yfinance/Polygon/Alpaca — no special-casing in domain, services, or app layers.

## Key Design Decision

ADR-0032 explicitly requires the demo seed flow to use the same normalized boundary as live providers. Demo mode is achieved by injecting `SeededMarketDataProvider` into `MarketSnapshotService` at composition root — no factory changes, no application-layer bypass. This prevents demo/production divergence.

## Seed Coverage

7 demo symbols covering all 5 `MarketRegime` outcomes (BULL, BEAR, RANGING, HIGH_VOLATILITY, LOW_VOLATILITY), enabling full regime interpretation demonstration without live data.

## Optional Timestamp Injection

`fetched_at` is injectable for deterministic test timestamps — required for reliable integration test assertions about provenance fields.

## Boundary Rule

Seeded data is still advisory. `is_advisory` always returns True. No event ledger writes. Fully deterministic and replayable.
