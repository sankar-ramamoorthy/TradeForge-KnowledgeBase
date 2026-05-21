---
title: Provider Boundary Model
type: processed-synthesis
status: processed
created: 2026-05-13
updated: 2026-05-21
source_issues: [TF-0042]
source_history:
  - knowledge/raw/archived/20260513-tf-0042-provider-boundary-interfaces-planning.md
  - knowledge/raw/archived/20260513-tf-0042-provider-boundary-interfaces-implementation.md
milestone: M9
related_adrs: [ADR-0010, ADR-0032]
tags: [market-context, provider-boundary, advisory, replayability, provenance]
related:
  - "[[MarketContext]]"
  - "[[EventLedger]]"
  - "[[HistoricalReconstruction]]"
  - "[[Provider Provenance Tracking]]"
  - "[[20260516-m10d-provider-capability-synthesis|M10D Provider Capability Model]]"
---

# Provider Boundary Model

## Stabilized Understanding

The provider boundary is the normalized, read-only domain boundary through
which external market data enters TradeForge as advisory context.

The boundary is architecturally prior to all concrete provider adapters. It
keeps provider SDKs, schemas, credentials, and failure modes subordinate to the
TradeForge decision architecture rather than allowing external data providers to
shape lifecycle state, workspace semantics, or canonical event truth.

## Processing Result

TF-0042 is processed as stable architectural knowledge for the M9 market
context layer. The source raw captures have already been archived, and this
processed artifact now preserves:

- the normalized provider-boundary contract
- the advisory/non-canonical authority rule
- the provenance fields required for replay integrity
- the ontology candidates introduced by the implementation
- the downstream issue relationships for M9 and later provider-capability work

No raw source movement is required in this pass because both TF-0042 source
captures already reside under `knowledge/raw/archived/`.

## Boundary Semantics

External providers may supply observations about market state. They do not
provide decisions, lifecycle intent, workflow authority, or canonical truth.

The provider boundary therefore enforces four rules:

- provider data is advisory context
- provider data is normalized before workspace or service consumption
- provider failures are explicit, not silently converted into empty context
- provider provenance travels with every snapshot

This aligns with the invariant that broker APIs and external systems are not
canonical truth. The [[EventLedger]] remains authoritative for TradeForge state.

## Key Runtime Concepts

| Concept | Layer | Semantic role |
|---|---|---|
| `MarketRegime` | domain | Advisory inferred regime classification |
| `ProviderProvenance` | domain | Per-snapshot origin and temporal traceability record |
| `PriceOHLCV` | domain | Normalized OHLCV price value object |
| `MarketSnapshot` | domain | Complete advisory snapshot: price, provenance, and regime |
| `MarketDataProvider` | domain port | Structural provider Protocol |
| `ProviderUnavailableError` | domain | Explicit provider failure type |

These concepts are stable enough for processed knowledge and runtime
implementation references. They are not yet promoted as individual canonical
entity pages in this KB. Promotion should occur only if later provider,
provenance, replay, or advisory-context work repeatedly needs independent
semantic definitions.

## Critical Invariant: `fetched_at` vs `data_as_of`

`ProviderProvenance` deliberately carries two timestamps:

- `fetched_at`: when the data was retrieved from the provider
- `data_as_of`: what market moment the data represents

This distinction is required for historical reconstruction. A decision made on
2026-05-13 may have been informed by a snapshot whose `data_as_of` is the prior
market close. Without this distinction, replay cannot determine what market
context was visible at the decision point.

For replay purposes, `data_as_of` is the historical market-data anchor.
`fetched_at` is the retrieval/audit anchor.

## Advisory Boundary Contract

`MarketSnapshot.is_advisory` always returns `True`.

This is a machine-readable authority marker. Any service, workspace projection,
or replay system consuming market snapshots must be able to distinguish advisory
provider context from canonical event history.

Provider data may influence operator awareness. It must not:

- write lifecycle events
- approve or reject trade plans
- execute trades
- mutate projections as if they were canonical state
- replace historical snapshots with live API calls during replay

## Replaceability Principle

`MarketDataProvider` is a structural Protocol rather than an inheritance-based
base class.

Concrete adapters such as yfinance, Polygon, Alpaca, and seeded demo providers
satisfy the Protocol by shape. This keeps provider replacement localized to
adapter construction and composition-root wiring rather than spreading SDK or
base-class dependencies into service and workspace layers.

## What This Boundary Prevents

- provider schema changes propagating into workspace projections
- external data becoming canonical Event Ledger entries
- provider-specific SDK coupling bleeding into domain or service layers
- silent stale or empty context on provider failure
- replay reconstruction depending on live provider APIs
- workspace surfaces treating inferred market context as authoritative state

## Ontology Observations

The note introduces a provider-boundary cluster rather than a single canonical
entity. The important semantic distinction is between:

- provider identity: who supplied external data
- provider capability: what external-data surface the provider can supply
- provider provenance: how a specific retrieved artifact is traced
- advisory snapshot: the normalized context artifact visible to the system

M10D later extends this by separating provider identity from typed provider
capabilities. That extension preserves TF-0042 rather than replacing it: TF-0042
defines the normalized price snapshot boundary, while M10D generalizes provider
selection around multiple external-data capabilities.

## Workflow Implications

Provider data supports situational awareness and workspace context acquisition.
It does not introduce a decision lifecycle transition and does not alter the
canonical lifecycle:

```text
Idea -> Thesis -> Plan -> Approval -> Execution -> Position -> Review
```

Provider snapshots may inform briefing, opportunity, thesis, exposure, or review
surfaces, but any material workflow transition must still pass through human
decision and lifecycle controls.

## Event Model Implications

TF-0042 intentionally does not add canonical events.

Market snapshots are not ledger facts. If advisory provider context needs to be
preserved for reconstruction, it should be stored as historical snapshot data or
captured through explicit advisory/provenance records without turning provider
payloads into canonical decision truth.

This preserves the event taxonomy rule that events are facts, not
interpretations.

## Replay Implications

The provider boundary enables replay-compatible market context by preserving
both origin and market-time semantics on each snapshot.

Replay systems should reconstruct visible provider context from historical
snapshots and provenance records. Replay must not call live providers to rebuild
past decision context.

TF-0052 later activates this implication by persisting advisory snapshots for
historical reconstruction.

## ADR And Architecture Implications

The processed source identifies ADR-0010 and ADR-0032 as governing context.
Within this KB pass, no new ADR is required:

- the boundary preserves event-ledger authority
- provider data remains advisory and non-canonical
- lifecycle and execution authority remain unchanged
- downstream provider adapters implement the boundary rather than redefining it

If future work changes provider snapshots from advisory context into canonical
facts, that would require a new ADR and explicit invariant review.

## Operational Synchronization

Current source traceability is complete for TF-0042:

- planning source archived at `knowledge/raw/archived/20260513-tf-0042-provider-boundary-interfaces-planning.md`
- implementation source archived at `knowledge/raw/archived/20260513-tf-0042-provider-boundary-interfaces-implementation.md`

Downstream processed artifacts already reference this boundary:

- TF-0043: normalized market snapshot service
- TF-0044 to TF-0046: concrete market data provider adapters
- TF-0050: provider provenance tracking
- TF-0051: seeded demo provider flow
- TF-0052: replay-compatible snapshot persistence
- M10D: provider capability model

No contradiction was found with the loaded invariants, architecture doctrine,
event taxonomy, or semantic governance rules.

## Canonical Readiness

The provider boundary model is stable as processed architectural knowledge.

Recommended next promotion, if recurrence continues:

- add glossary definitions for `ProviderProvenance`, `MarketSnapshot`, and
  `MarketDataProvider`
- consider a topic page for provider boundaries across price, fundamentals, and
  advisory AI providers
- consider entity pages only for concepts that need durable independent meaning
  outside this processed synthesis

Do not prematurely promote every runtime value object into an ontology entity.
The canonical semantic center is the advisory provider boundary, not the
implementation detail of each class.

## Downstream Activation

- TF-0043: services-layer snapshot normalization
- TF-0044 to TF-0046: concrete provider adapters implementing this Protocol
- TF-0047: workspace context overlays consuming normalized snapshots
- TF-0052: replay-compatible snapshot persistence using `data_as_of` as the historical anchor
- M10D: typed provider capability registry extending provider selection beyond OHLCV
