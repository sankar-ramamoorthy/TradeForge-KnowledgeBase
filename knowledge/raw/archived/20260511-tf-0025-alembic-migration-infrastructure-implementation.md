---
title: TF-0025 Alembic Migration Infrastructure Implementation
type: raw-capture
status: raw
created: 2026-05-11
issue: TF-0025
milestone: M7
branch: feature/tf-0025-alembic-migrations
related:
  - ADR 0018
  - ADR 0019
  - Migration Infrastructure
---

# TF-0025 Alembic Migration Infrastructure Implementation

## Implementation Summary

TF-0025 added Alembic migration infrastructure for Postgres-backed runtime schema management.

Runtime changes:
- Added `alembic` and `psycopg[binary]` runtime dependencies and updated `uv.lock`.
- Added `alembic.ini`.
- Added `migrations/env.py`, `migrations/script.py.mako`, and migration README.
- Added deterministic bootstrap revision `20260511_0001_bootstrap_runtime_schema.py`.
- Added ADR 0019 for projection persistence boundaries.
- Added focused migration infrastructure tests.
- Updated README with the migration command.
- Marked TF-0025 complete in runtime roadmap and issue register.

## Architecture Decisions

The bootstrap migration intentionally creates no domain tables. Event ledger schema remains scoped to TF-0026. Projection persistence remains deferred and constrained by ADR 0019.

Alembic reads the existing Postgres connection settings from the infrastructure layer and converts generic Postgres URLs to the SQLAlchemy `postgresql+psycopg://` dialect only inside migration execution.

## Replay Implications

Replay behavior is unchanged. Migration tooling does not create canonical truth. Physical schema is operational infrastructure; Event Ledger history and deterministic rules remain replay authority.

## Lifecycle Implications

No lifecycle stages, transition validators, or orchestration behavior changed.

## Validation Executed

- `uv lock`
- `uv run pytest`
- `uv run ruff check .`
- `uv run mypy src tests`
- `docker compose config`
- `uv run alembic upgrade head`
- `uv run alembic current`

Alembic upgraded the local Postgres database to revision `20260511_0001`.

## Follow-Up Work

- TF-0026 should add the append-only Postgres Event Store adapter and event ledger schema.
- Projection persistence should remain deferred until a scoped issue with rebuild semantics and source-event traceability tests.
