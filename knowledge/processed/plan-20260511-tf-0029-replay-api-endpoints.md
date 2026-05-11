---
title: Plan - TF-0029 Replay API Endpoints
type: processed-planning
status: processed
created: 2026-05-11
source: knowledge/raw/archived/20260511-plan-tf-0029-replay-api-endpoints.md
issue: TF-0029
milestone: M7
branch: feature/tf-0029-replay-api-endpoints
related:
  - "[[ReplayTimeline]]"
  - "[[HistoricalReconstruction]]"
  - "[[Event Store Port]]"
  - "[[Event Ledger]]"
---

# Plan - TF-0029 Replay API Endpoints

## Stabilized Planning Summary

TF-0029 introduces replay HTTP endpoints as app-layer adapters over the existing replay services. The HTTP layer may expose replay read models, but replay derivation remains owned by deterministic replay components that read canonical event history through the existing runtime boundaries.

## Semantic Observations

- replay API output is derived reconstruction, not canonical truth;
- replay routes must read the same event history that other runtime workflow endpoints append;
- historical reconstruction and replay timeline are distinct replay read models and should remain distinct in the API boundary;
- replay APIs must not call live market APIs, current AI output, or mutable UI state.

## Architecture Boundaries

The implementation should preserve these boundaries:

- FastAPI routes: request handling and response translation only.
- `HistoricalReconstructionPipeline`: derived replay reconstruction from canonical event history.
- `ReplayTimelineService`: deterministic replay timeline read path.
- [[Event Store Port]]: only read boundary used by replay API behavior.
- [[Event Ledger]]: canonical truth remains immutable event history, not replay API output.

TF-0029 should not add:

- route-local replay logic;
- direct Postgres access from HTTP handlers;
- replay workspace UI behavior;
- AI narration;
- hidden state separate from the shared event-store-backed runtime services.

## ADR Assessment

No new ADR is required. Existing runtime ADRs already define the necessary authority split:

- ADR 0008 defines replay as deterministic reconstruction from Event Ledger history.
- ADR 0014 defines replay as a first-class derived interaction model.
- ADR 0020 defines FastAPI as an app-layer adapter and scopes replay APIs to this issue.

## Runtime Implementation Expectations

Runtime implementation should add:

- shared app wiring so lifecycle and replay read the same runtime event history;
- response DTOs for historical reconstruction and replay timeline outputs;
- `GET /replay` and `GET /replay/timeline` route handlers that delegate to replay services only;
- tests covering reconstruction output, timeline output, deterministic repeated reads, and shared runtime event history;
- runtime and KB synchronization after verification.

## Processing Result

This planning capture stabilizes API-boundary intent and replay authority discipline only. It does not introduce new replay ontology, new event taxonomy, or new workflow doctrine.
