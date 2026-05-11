---
title: Plan - TF-0026 Postgres Event Ledger
type: processed-planning
status: processed
created: 2026-05-11
source: knowledge/raw/archived/20260511-tf-0026-postgres-event-ledger-planning.md
issue: TF-0026
milestone: M7
branch: feature/tf-0026-postgres-event-ledger
related:
  - [[Event Ledger]]
  - [[Event Store Port]]
  - [[Event]]
  - [[Replay System]]
---

# Plan - TF-0026 Postgres Event Ledger

## Stabilized Planning Summary

TF-0026 is the first issue where durable Postgres persistence begins carrying canonical [[Event Ledger]] history. The implementation should introduce a Postgres adapter behind the existing [[Event Store Port]] and persist immutable event facts without changing event meaning, lifecycle authority, or replay semantics.

## Semantic Observations

- Durable persistence begins carrying canonical event history in this issue.
- [[Event Store Port]] discipline becomes more important, not less, once canonical history is stored in Postgres.
- Physical table design must support replay ordering and EventEnvelope reconstruction without redefining event semantics.
- Database-level append-only guards are appropriate because immutability is an invariant, not just an adapter preference.

## Architecture Boundaries

The implementation should preserve these boundaries:

- Event Ledger: canonical truth.
- Event Store Port: runtime boundary for appending and reading events.
- Postgres adapter: infrastructure implementation only.
- Domain and services layers: no Postgres, SQLAlchemy, or migration coupling.
- Projection persistence: separate, deferred, and non-authoritative.

TF-0026 should not introduce event streaming infrastructure, projection storage, or new event taxonomy.

## ADR Assessment

No new ADR is required.

ADR 0001, ADR 0003, and ADR 0018 already authorize event-sourced canonical truth, event taxonomy discipline, and the Postgres persistence boundary. TF-0026 implements accepted architecture rather than changing it.

## Runtime Implementation Expectations

Runtime implementation should add:

- Alembic migration for the event ledger table and append-only guards;
- a Postgres Event Store adapter in infrastructure;
- deterministic read ordering by immutable ledger sequence;
- lossless EventEnvelope reconstruction semantics;
- focused tests for serialization, ordering, append-only behavior, and port adherence;
- runtime documentation updates only where adapter-boundary clarification is useful.

## Processing Result

This planning capture does not introduce new ontology, workflow, playbook, or doctrine structures. The stable knowledge is durable-canonical-persistence boundary clarification for the first Postgres-backed Event Ledger implementation.
