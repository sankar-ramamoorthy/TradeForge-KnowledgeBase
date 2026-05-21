---
title: TF-0042 Provider Boundary Interfaces — Planning Capture
type: raw-planning
status: raw
created: 2026-05-13
issue: TF-0042
milestone: M9
---

# TF-0042: Define Provider Boundary Interfaces — Planning Capture

## Issue Goal

Define the normalized domain-layer boundary interfaces for external market data providers.

This is the foundational architectural work that all M9 provider adapters (TF-0044 yfinance, TF-0045 Polygon, TF-0046 Alpaca) must depend on.

## Dominant Task Category

External API boundary / Infrastructure integration.

Context bundle: INVARIANTS + ADR-0010 + ADR-0032.

## Architecture Layer Affected

- `src/domain/market/` — new domain module (pure, no infrastructure coupling)

No service, app, or infrastructure changes in this issue.

## Key Design Decisions

### 1. Market snapshots are NOT events

Market snapshots do not use the canonical event domain system (`EventDomain`, `EventEnvelope`).

Per ADR-0032: external market data is advisory context, not canonical business facts.

Injecting provider data into the event ledger would violate event integrity.

### 2. Provenance is mandatory on every snapshot

Every `MarketSnapshot` must carry a `ProviderProvenance` capturing:
- `provider_id` — stable string (e.g. "yfinance", "alpaca", "polygon")
- `provider_version` — adapter version string
- `fetched_at` — when the data was retrieved
- `data_as_of` — what moment in time the data represents

The `fetched_at` / `data_as_of` distinction is critical for replay:
- `fetched_at` = when we obtained the data (real-world fetch time)
- `data_as_of` = the market moment the data represents (e.g. close of 2026-05-12)

Without this, historical reconstruction cannot determine what market context existed at a decision point.

### 3. Protocol (not ABC) for the provider port

Consistent with existing `EventStore` Protocol pattern.

Allows test stubs and demo providers to satisfy the interface via structural subtyping, without requiring inheritance.

### 4. Decimal for price fields

Financial data requires decimal precision. `float` arithmetic introduces rounding errors that, while acceptable for display, are inappropriate for values that may appear in comparison or threshold logic.

### 5. MarketRegime is advisory, not canonical

`MarketRegime` classification on a snapshot is inferred context. It is derived from provider data interpretation, not from lifecycle events or deterministic rules.

Always labeled advisory in output.

### 6. is_advisory property as explicit contract

`MarketSnapshot.is_advisory` always returns `True`.

This is a machine-readable boundary marker — any workspace or service consuming snapshots must never treat them as canonical state.

## Entities Introduced

- `MarketRegime` (StrEnum) — inferred contextual classification
- `ProviderProvenance` (frozen dataclass) — who/when/which version
- `PriceOHLCV` (frozen dataclass) — normalized open/high/low/close/volume
- `MarketSnapshot` (frozen dataclass) — complete advisory snapshot
- `MarketDataProvider` (Protocol) — provider port interface

## Replay Implication

These interfaces enable TF-0052 (replay-compatible snapshot persistence) to store advisory market context with explicit provenance, supporting future historical reconstruction that knows what market data was visible at decision time.

Not implementing persistence here — that is TF-0052.

## Lifecycle Implication

None. These are read-only advisory interfaces. No lifecycle transitions are triggered by provider data.

## Event Model Implication

None. Provider data is not written to the event ledger.

## Testing Strategy

- Domain model immutability tests
- Provenance validation tests (empty provider_id rejected)
- Protocol structural subtyping test (stub adapter satisfies interface)
- `is_advisory` contract test
- Regime default test

## Out of Scope

- Actual provider adapters (TF-0044 to TF-0046)
- Workspace overlays (TF-0047)
- Market regime interpretation service (TF-0048)
- Snapshot persistence (TF-0052)
- Demo seed flow (TF-0051)

## Unresolved Questions

None for this issue. Boundary is clear.
