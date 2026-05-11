---
title: TF-0026 Postgres Event Ledger Implementation
type: raw-capture
status: raw
created: 2026-05-11
issue: TF-0026
milestone: M7
branch: feature/tf-0026-postgres-event-ledger
related:
  - ADR 0001
  - ADR 0003
  - ADR 0018
  - Event Ledger
  - Event Store Port
---

# TF-0026 Postgres Event Ledger Implementation

## Implementation Summary

TF-0026 implemented durable Postgres persistence for the canonical Event Ledger behind the existing EventStore port.

Runtime changes:
- Added Alembic revision `20260511_0002_create_event_ledger.py`.
- Created `event_ledger` table with deterministic `ledger_sequence` replay ordering.
- Added database trigger/function to reject direct UPDATE and DELETE operations.
- Added `PostgresEventStore` infrastructure adapter.
- Exported `PostgresEventStore` from `src.infrastructure.event_store`.
- Added focused adapter, serialization, migration, and port-boundary tests.
- Updated README with the EventStore port boundary note.
- Marked TF-0026 complete in the runtime issue register and roadmap.

## Architecture Decisions

The adapter stores EventEnvelope fields directly as event facts:
- event type
- occurred timestamp
- persona id
- optional workspace id
- entity references
- payload
- provenance

Read ordering is by immutable ledger sequence rather than event timestamp. This preserves deterministic replay order for append history.

Database-level append-only guards were added because Event Ledger immutability is an invariant, not only an adapter convention.

## Replay Implications

Replay can now consume durable event history through the same EventStore port. No replay service behavior changed. The physical table supports replay by preserving append order and reconstructing EventEnvelope objects.

## Lifecycle Implications

No lifecycle semantics changed. Lifecycle orchestration can continue to write through EventStore without knowing whether the backing adapter is in-memory or Postgres.

## Validation Executed

- `uv run pytest`
- `uv run ruff check .`
- `uv run mypy src tests`
- `docker compose config`
- `uv run alembic upgrade head`
- `uv run alembic current`
- Live PostgresEventStore append/read/mutation-guard check against local Postgres.

Alembic upgraded local Postgres to revision `20260511_0002`.

## Follow-Up Work

- TF-0027 can add the FastAPI runtime boundary.
- Later app wiring should inject the Postgres adapter behind the EventStore port without importing infrastructure into domain/services logic.
- Event streaming remains out of scope.
