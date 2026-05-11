---
title: Plan - TF-0024 Postgres Persistence Layer
type: processed-planning
status: processed
created: 2026-05-11
source: knowledge/raw/archived/20260511-tf-0024-postgres-persistence-planning.md
issue: TF-0024
milestone: M7
branch: feature/tf-0024-postgres-persistence
related:
  - [[Event Ledger]]
  - [[Event Store Port]]
  - [[Projection]]
  - [[Replay System]]
---

# Plan - TF-0024 Postgres Persistence Layer

## Stabilized Planning Summary

TF-0024 establishes Postgres as local runtime infrastructure for M7 without changing semantic authority. The issue sets up local database availability, infrastructure-scoped connection configuration, and the ADR needed to preserve the event-store boundary before durable adapters are implemented.

## Semantic Observations

- Postgres is infrastructure, not a semantic layer.
- [[Event Ledger]] authority still comes from immutable events, not from a database choice.
- [[Event Store Port]] remains the only acceptable boundary between runtime orchestration and persistence adapters.
- Replay semantics remain unchanged because persistence enablement does not redefine event meaning, lifecycle authority, or workspace behavior.

## Architecture Boundaries

The implementation should preserve these boundaries:

- Event Ledger: canonical truth.
- Event Store Port: persistence boundary.
- Postgres: infrastructure concern only.
- Domain and services layers: no database-specific imports.
- Replay System: deterministic reconstruction from event history and rules, not from database semantics.

TF-0024 should not implement the Postgres event ledger adapter, migration infrastructure, or projection persistence.

## ADR Assessment

ADR 0018 is required and should be materialized during TF-0024 because durable persistence strategy is future-facing architecture, not a hidden implementation detail.

The accepted boundary is:

```text
domain/services -> Event Store port -> infrastructure adapters -> Postgres
```

## Runtime Implementation Expectations

Runtime implementation should add:

- local Postgres service wiring in Docker Compose;
- infrastructure-only connection settings;
- README instructions for local runtime setup;
- focused tests for Compose wiring and connection settings;
- runtime ADR 0018 documenting persistence boundary and deferred follow-up issues.

## Processing Result

This planning capture does not introduce new ontology, workflow, playbook, or doctrine structures. The stable knowledge is boundary clarification for M7 infrastructure work and alignment with runtime ADR 0018.
