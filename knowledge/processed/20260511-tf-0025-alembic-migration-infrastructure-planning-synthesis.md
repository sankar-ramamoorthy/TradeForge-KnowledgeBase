---
title: TF-0025 Alembic Migration Infrastructure Planning Synthesis
type: processed-synthesis
status: processed
created: 2026-05-11
source: knowledge/raw/20260511-tf-0025-alembic-migration-infrastructure-planning.md
issue: TF-0025
milestone: M7
---

# TF-0025 Alembic Migration Infrastructure Planning Synthesis

## Stabilized Understanding

Migration infrastructure is operational tooling for physical database evolution. It is not a semantic authority and must not encode domain meaning outside ADR-governed schema decisions.

## Boundary Rule

Alembic may manage tables in Postgres, but runtime truth remains the Event Ledger. Projection persistence, when introduced, must remain rebuildable and discardable.

## ADR Outcome

ADR 0019 should be materialized during TF-0025 because migration tooling creates the mechanism that future projection tables will use. The ADR should prevent projection rows from being treated as canonical truth.

## Implementation Constraint

The initial migration should be deterministic and intentionally avoid creating event-ledger or projection schema. Those belong to scoped follow-up issues.
