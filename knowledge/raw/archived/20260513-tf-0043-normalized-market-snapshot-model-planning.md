---
title: TF-0043 Normalized Market Snapshot Model — Planning Capture
type: raw-planning
status: raw
created: 2026-05-13
issue: TF-0043
milestone: M9
---

# TF-0043: Implement Normalized Market Snapshot Model — Planning Capture

## Issue Goal

Implement the services-layer market context model that sits between provider
adapters (TF-0044 to TF-0046) and workspace overlays (TF-0047).

This layer orchestrates provider calls, handles partial failures gracefully,
and returns a normalized MarketContextResult that workspace overlays can
consume without knowing which provider served the data.

## Dominant Task Category

Services layer orchestration / external API boundary normalization.

Context bundle: INVARIANTS + ADR-0010 + ADR-0032 + provider boundary model (TF-0042).

## Architecture Layer Affected

- `src/services/market/` — new services module
  - `context.py` — request/result value objects
  - `snapshot_service.py` — MarketSnapshotService orchestration class
  - `__init__.py`

No domain layer changes. No infrastructure changes.

## Key Design Decisions

### 1. MarketContextResult captures partial failures

`fetch_context` must never raise for individual symbol failures.

Workspace overlays (TF-0047) must be able to display partial results when
some symbols are unavailable. Raising on the first failure would make the
entire workspace context surface blank on any provider instability.

Design:
- `available` — tuple of successfully fetched MarketSnapshot objects
- `unavailable_symbols` — tuple of symbols where ProviderUnavailableError occurred
- Callers can inspect `unavailable_symbols` and surface advisory warnings

### 2. fetch_snapshot re-raises ProviderUnavailableError

For single-symbol callers that explicitly need the data, silence is wrong.
`fetch_snapshot` propagates the provider error directly.

This preserves the explicit failure handling rule from ADR-0032.

### 3. MarketContextResult.authority is always ADVISORY

Consistent with MarketSnapshot.is_advisory. No code path can produce
a MarketContextResult with canonical authority.

### 4. snapshot_for convenience lookup

`MarketContextResult.snapshot_for(symbol)` returns the snapshot for a symbol
if available, or None. Avoids callers having to iterate `available` manually.

### 5. persona_id on MarketContextRequest (optional)

Not used in M9 normalization — present for future persona-shaped context
weighting in M9/M10. Optional field, None by default. Does not affect
current normalization logic.

### 6. No caching in this service

MarketSnapshotService is stateless. Caching is an infrastructure concern
for future issues. Services layer orchestrates — it does not cache.

## Entities Introduced

- `MarketContextAuthority` (StrEnum) — ADVISORY only
- `MarketContextRequest` (frozen dataclass) — symbols + optional persona_id
- `SymbolFetchResult` (frozen dataclass) — per-symbol success or failure record
- `MarketContextResult` (frozen dataclass) — collection of snapshots + failures + metadata
- `MarketSnapshotService` (class) — orchestrates provider, returns MarketContextResult

## Replay Implication

`MarketContextResult` is derived advisory context. It is not persisted here.

TF-0052 will use the provenance fields already on individual MarketSnapshot
objects (which come from TF-0042) to persist snapshots for historical replay.

## Lifecycle Implication

None. Read-only advisory context.

## Event Model Implication

None. No events written.

## Testing Strategy

- MarketContextRequest immutability and symbol validation
- MarketContextResult: available/unavailable tracking, snapshot_for lookup
- MarketSnapshotService.fetch_context: all available, partial failure, total failure
- MarketSnapshotService.fetch_snapshot: success and propagated failure
- Authority is always ADVISORY

## Out of Scope

- Actual provider adapters (TF-0044 to TF-0046)
- Workspace overlays (TF-0047)
- Snapshot persistence (TF-0052)
- Caching infrastructure
