---
title: Implemented - TF-0026 Postgres Event Ledger
type: processed-implementation
status: processed
created: 2026-05-11
source: knowledge/raw/archived/20260511-tf-0026-postgres-event-ledger-implementation.md
issue: TF-0026
milestone: M7
branch: feature/tf-0026-postgres-event-ledger
related:
  - [[Event Ledger]]
  - [[Event Store Port]]
  - [[Event]]
  - [[Replay System]]
---

# Implemented - TF-0026 Postgres Event Ledger

## Stable Implementation Knowledge

TF-0026 implemented a durable Postgres adapter for the canonical [[Event Ledger]] behind the existing [[Event Store Port]] and added database-level append-only guards to reinforce replay integrity.

## Architectural Meaning

The implementation stabilizes durable canonical persistence without changing semantic authority:

- Event Ledger remains canonical truth.
- Event Store Port remains the runtime boundary.
- Domain and services layers remain free of Postgres and SQLAlchemy imports.
- Replay remains event-backed and deterministic.
- Projection persistence remains separate and non-authoritative.

## Scope Preservation

TF-0026 intentionally did not add:

- event streaming infrastructure;
- projection persistence;
- lifecycle semantic changes;
- new event taxonomy;
- API-layer behavior.

The implementation stores EventEnvelope facts directly and orders replay reads by immutable ledger sequence rather than timestamp-only ordering.

## Validation

Runtime validation completed successfully:

- `uv run pytest`
- `uv run ruff check .`
- `uv run mypy src tests`
- `docker compose config`
- `uv run alembic upgrade head`
- `uv run alembic current`
- live `PostgresEventStore` append/read/mutation-guard check against local Postgres

The archived implementation note records that local Postgres was upgraded to Alembic revision `20260511_0002`.

## Operational Synchronization

Runtime alignment confirmed on 2026-05-11:

- runtime issue register marks TF-0026 Done
- runtime roadmap marks TF-0026 Done within planned M7 infrastructure work
- runtime docs record live adapter verification against local Postgres

No contradiction was found with [[INVARIANTS]], [[ARCHITECTURE]], [[GLOSSARY]], [[EVENT_TAXONOMY]], or [[UX_DOCTRINE]].
