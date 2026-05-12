---
title: TF-0029 Replay API Endpoints Implementation Capture
type: raw-note
status: archived
created: 2026-05-11
issue: TF-0029
milestone: M7
branch: feature/tf-0029-replay-api-endpoints
---

# TF-0029 Replay API Endpoints Implementation Capture

## Runtime Outcome

TF-0029 added:

- `GET /replay` for historical reconstruction output;
- `GET /replay/timeline` for deterministic replay timeline output.

The app factory now provisions a shared in-memory event store by default and attaches:

- `LifecycleOrchestrationService`
- `ReplayTimelineService`
- `HistoricalReconstructionPipeline`

to the same app runtime so replay reads the same event history that lifecycle transitions append.

## Boundary Preservation

- FastAPI remains request/response translation only.
- Replay reconstruction stays in services/domain layers.
- Replay output remains derived state, not canonical truth.
- No live market API calls were introduced.
- No replay endpoint mutates lifecycle state or appends events.

## Validation Executed

- `uv run pytest tests\test_fastapi_runtime.py tests\test_replay_projection_service.py tests\test_replay_timeline_service.py tests\test_historical_reconstruction_pipeline.py`
- `uv run pytest`
- `uv run ruff check .`
- `uv run mypy src tests`

Focused verification passed with `23` targeted tests.

Full verification passed with `169` total tests.

## Synchronization Notes

- runtime issue register should mark TF-0029 Done;
- runtime roadmap should annotate TF-0028 and TF-0029 as done in the M7 API layer;
- KB issue and milestone indexes should record replay APIs as derived HTTP adapters over existing replay services.
