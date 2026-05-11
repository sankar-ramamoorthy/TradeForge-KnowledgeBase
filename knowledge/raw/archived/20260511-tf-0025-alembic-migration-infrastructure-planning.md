---
title: TF-0025 Alembic Migration Infrastructure Planning
type: raw-capture
status: raw
created: 2026-05-11
issue: TF-0025
milestone: M7
branch: feature/tf-0025-alembic-migrations
related:
  - Runtime KB Development Loop
  - ADR 0018
  - ADR 0019
---

# TF-0025 Alembic Migration Infrastructure Planning

## Issue Context

TF-0025 adds Alembic migration infrastructure for Postgres-backed runtime tables.

Issue metadata:
- Issue ID: TF-0025
- Milestone: M7
- Title: Add Alembic migration infrastructure
- Affected layer: infrastructure
- Linked ADRs: ADR 0018, ADR 0019
- Impacted invariants: Replay, Layer Separation

## Planning Summary

The runtime has local Postgres infrastructure from TF-0024. TF-0025 should introduce deterministic schema migration tooling without introducing event-ledger persistence behavior or projection persistence behavior.

## Architecture Reasoning

Alembic is an infrastructure tool. It may manage physical database schema, but it does not define Event Ledger semantics, lifecycle authority, replay behavior, workspace meaning, or projection truth.

The safest initial migration is a deterministic bootstrap revision that does not create domain tables yet. Event ledger tables belong to TF-0026. Projection persistence belongs to later projection persistence work governed by ADR 0019.

## ADR Decision

ADR required: yes.

Reason: TF-0025 links ADR 0019, and projection persistence introduces a source-of-truth risk even though this issue does not implement projection storage. ADR 0019 should be materialized to constrain future projection migrations before migration tooling exists.

Proposed ADR: `DOCS/adr/0019-projection-persistence-architecture.md`.

## Event And Replay Implications

No new event types are introduced. Replay behavior is unchanged. Migration tooling must not make database schema or projection rows canonical truth. Replay remains dependent on immutable events and deterministic rules.

## Lifecycle Implications

No lifecycle transitions, validators, or orchestration behavior changes.

## Implementation Boundaries

In scope:
- Add Alembic dependency and lock update.
- Add Alembic config and migration environment.
- Wire migration database URL through existing Postgres settings.
- Add deterministic bootstrap revision with no domain tables.
- Document migration command.
- Add focused tests for migration files and boundary constraints.
- Update issue register and roadmap after verification.

Out of scope:
- Event ledger table implementation.
- Postgres EventStore adapter.
- Projection tables.
- Production deployment process.

## Open Questions

No semantic blocker identified. Dependency resolution may require network access through the approved escalation path.
