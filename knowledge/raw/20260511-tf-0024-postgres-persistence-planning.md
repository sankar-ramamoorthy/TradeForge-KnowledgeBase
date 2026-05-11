---
title: TF-0024 Postgres Persistence Planning
type: raw-capture
status: raw
created: 2026-05-11
issue: TF-0024
milestone: M7
branch: feature/tf-0024-postgres-persistence
related:
  - Runtime KB Development Loop
  - ADR 0018
---

# TF-0024 Postgres Persistence Planning

## Issue Context

TF-0024 adds the Postgres persistence foundation for M7 runtime infrastructure.

Issue metadata:
- Issue ID: TF-0024
- Milestone: M7
- Title: Add Postgres persistence layer
- Affected layer: infrastructure
- Linked ADR: ADR 0018
- Impacted invariants: Event Sourcing, Replay, Layer Separation

## Planning Summary

The runtime is moving from in-memory foundational behavior toward durable operational infrastructure. TF-0024 should establish Postgres as a local runtime dependency and define the persistence boundary without implementing the canonical event ledger adapter, migrations, or projection persistence.

## Architecture Reasoning

Postgres is infrastructure only. It must not define domain semantics, event meaning, lifecycle transitions, workspace behavior, persona interpretation, or replay rules.

The correct boundary is:

```text
Domain EventStore port
  -> infrastructure adapters
  -> Postgres durable storage in later issues
```

TF-0024 prepares this boundary by adding local Postgres availability and runtime connection configuration. TF-0026 will implement the event ledger adapter behind the existing EventStore port. TF-0025 will add migration infrastructure.

## ADR Decision

ADR required: yes.

Reason: adding Postgres introduces a durable persistence strategy and source-of-truth boundary risk. Runtime ADR 0018 is referenced by the issue register but does not yet exist as a materialized ADR file.

Proposed ADR: `DOCS/adr/0018-postgres-event-store-persistence.md`.

## Replay Implications

Replay is not changed in this issue. The design must preserve that replay reconstructs state from immutable events and deterministic rules. Postgres enables future durable event history but does not itself become a semantic authority.

## Lifecycle Implications

No lifecycle stages or transition rules change. Lifecycle authority remains in the Decision Lifecycle Engine.

## Implementation Boundaries

In scope:
- local Postgres service in Docker Compose
- Postgres connection settings in infrastructure
- ADR 0018 materialization
- README setup notes
- focused tests for config and Compose wiring

Out of scope:
- Alembic migrations
- Postgres event ledger adapter
- projection persistence
- event schema changes
- lifecycle API or UI changes

## Open Questions

No semantic blockers identified. Dependency additions should be avoided unless a runtime connection implementation actually needs them.
