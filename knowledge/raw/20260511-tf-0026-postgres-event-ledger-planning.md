---
title: TF-0026 Postgres Event Ledger Planning
type: raw-capture
status: raw
created: 2026-05-11
issue: TF-0026
milestone: M7
branch: feature/tf-0026-postgres-event-ledger
related:
  - Runtime KB Development Loop
  - ADR 0001
  - ADR 0003
  - ADR 0018
---

# TF-0026 Postgres Event Ledger Planning

## Issue Context

TF-0026 persists the canonical Event Ledger through a Postgres EventStore adapter.

Issue metadata:
- Issue ID: TF-0026
- Milestone: M7
- Title: Persist Canonical Event Ledger
- Affected layer: infrastructure
- Linked ADRs: ADR 0001, ADR 0003, ADR 0018
- Impacted invariants: Event Sourcing, Event Integrity, Replay

## Planning Summary

The runtime already has Postgres availability and Alembic migration infrastructure. TF-0026 should create the first canonical event-ledger schema and a Postgres adapter that satisfies the existing EventStore port without changing domain event semantics.

## Architecture Reasoning

The EventStore port remains the boundary. Domain and services code continue to depend on `EventStore`, not SQLAlchemy, Postgres, or migration concepts.

The Postgres table records EventEnvelope fields as facts:
- deterministic ledger sequence
- event type
- event timestamp
- persona id
- optional workspace id
- entity references
- payload
- provenance
- recorded timestamp

The table should include database-level guards against UPDATE and DELETE to reinforce append-only behavior beyond adapter method shape.

## ADR Decision

ADR required: no new ADR.

Reason: ADR 0001, ADR 0003, and ADR 0018 already authorize the event-sourced model, canonical event taxonomy, and Postgres persistence boundary. TF-0026 implements that accepted architecture without introducing a new persistence strategy or semantic model.

## Event And Replay Implications

No new event types are introduced. This issue changes physical persistence of existing EventEnvelope facts. Replay should become durable because read order is determined by ledger sequence, not by mutable current state.

Invalid patterns to avoid:
- using database rows as mutable current-state records
- exposing update/delete/truncate methods
- deriving event meaning from table design
- storing projections in the event ledger table

## Lifecycle Implications

No lifecycle transition rules change. Lifecycle orchestration can use the same EventStore port with a durable adapter in later wiring.

## Implementation Boundaries

In scope:
- Add Alembic migration for event ledger table and append-only guards.
- Add Postgres EventStore adapter in infrastructure.
- Preserve EventEnvelope reconstruction semantics.
- Export adapter from infrastructure event_store package.
- Add focused tests for schema, serialization, port behavior, deterministic read ordering, and no mutation API.
- Update README with migration/adapter boundary notes if useful.
- Update issue register and roadmap after verification.

Out of scope:
- Event streaming infrastructure.
- API wiring.
- Production deployment hardening.
- Projection persistence.
- Lifecycle API endpoints.

## Open Questions

No semantic blocker identified. Live adapter verification requires local Postgres to be available and migrated.
