---
title: TF-0043 Synthesis — Normalized Market Snapshot Service
type: processed-synthesis
status: processed
issue: TF-0043
milestone: M9
created: 2026-05-13
source_history:
  - knowledge/raw/20260513-tf-0043-normalized-market-snapshot-model-planning.md
  - knowledge/raw/20260513-tf-0043-normalized-market-snapshot-model-implementation.md
tags: [market-context, services-layer, advisory, partial-failure, M9]
related:
  - "[[Provider Boundary Model]]"
  - "[[Provider Provenance Tracking]]"
---

# TF-0043 Synthesis — Normalized Market Snapshot Service

## Stabilized Understanding

The `MarketSnapshotService` is a services-layer orchestrator that sits between provider adapters and workspace overlays. It normalizes provider calls and handles partial failures gracefully so workspaces always receive a coherent result, even when some symbols are unavailable.

## Key Design Decisions Preserved

- `fetch_context` never raises for individual symbol failures — `MarketContextResult` captures partial failures as first-class data via `unavailable_symbols` and `SymbolFetchResult` records.
- `fetch_snapshot` re-raises `ProviderUnavailableError` for single-symbol callers who explicitly need the data.
- `MarketContextResult.authority` is always `ADVISORY` — no code path can produce a non-advisory result.
- `persona_id` is an optional placeholder on `MarketContextRequest` reserved for future persona-shaped context weighting in M10.
- The service is stateless — no caching. Caching is deferred as an infrastructure concern.

## Boundary Rule

`MarketSnapshotService` orchestrates provider calls and normalizes results. It does not own lifecycle authority, event ledger writes, or canonical state. Workspace overlays consume its output as advisory context only.

## Replay Implication

`MarketContextResult` is derived advisory context, not persisted here. TF-0052 uses `ProviderProvenance.data_as_of` on individual snapshots for historical reconstruction.
