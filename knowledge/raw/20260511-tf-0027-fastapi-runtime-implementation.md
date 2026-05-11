---
title: TF-0027 FastAPI Runtime Implementation
type: raw-capture
status: raw
created: 2026-05-11
issue: TF-0027
milestone: M7
branch: feature/tf-0027-fastapi-runtime
related:
  - ADR 0020
  - FastAPI Runtime Boundary
---

# TF-0027 FastAPI Runtime Implementation

## Implementation Summary

TF-0027 added FastAPI as the HTTP application runtime boundary.

Runtime changes:
- Added `fastapi[standard]` dependency and updated `uv.lock`.
- Added ADR 0020 for the FastAPI runtime boundary.
- Added `src/app/api/application.py` with a shared app factory.
- Added `src/app/api/routes.py` with a minimal `/health` operational route.
- Added app package export through `src/app/api/__init__.py`.
- Added FastAPI runtime tests using TestClient.
- Documented local FastAPI run command in README.
- Marked TF-0027 complete in runtime roadmap and issue register.

## Architecture Decisions

The `/health` route returns operational boundary metadata only. It does not expose lifecycle, replay, workspace, persistence, or domain behavior.

Endpoint-specific work remains deferred:
- TF-0028 lifecycle API endpoints
- TF-0029 replay API endpoints
- TF-0030 workspace projection APIs
- TF-0034 authentication/session model

## Event And Replay Implications

No event types were added. No event store behavior changed. Replay remains event-backed and deterministic.

## Lifecycle Implications

No lifecycle transition behavior was implemented. HTTP handlers do not own lifecycle rules and future lifecycle endpoints must delegate to services.

## Validation Executed

- `uv lock`
- `uv run pytest`
- `uv run ruff check .`
- `uv run mypy src tests`
- `docker compose config`
- `uv run uvicorn src.app.api.application:app --host 127.0.0.1 --port 8000`

The full runtime test suite passed with 163 tests. The local FastAPI runtime was verified through `/health` on `http://127.0.0.1:8000`. The local FastAPI runtime was verified through `/health` on `http://127.0.0.1:8000`.

## Follow-Up Work

TF-0028 can add lifecycle API endpoints by registering route modules into the app factory and delegating transition behavior to lifecycle services.


