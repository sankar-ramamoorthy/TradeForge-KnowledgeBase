---
title: TF-0068 Planning - Advisory Provenance Tracking
type: raw-planning
status: raw
created: 2026-05-19
issue: TF-0068
milestone: M11
---

# TF-0068 Planning - Advisory Provenance Tracking

## Scope

Implement non-canonical advisory provenance tracking for generated advisory responses.

## Design

- Add an advisory provenance record and store protocol to `src/domain/advisory`.
- Add an in-memory infrastructure adapter for local runtime and tests.
- Add a services-layer query/recording wrapper.
- Query by request ID, artifact kind, and source reference.

## Boundaries

- Advisory provenance records are not event-ledger facts.
- No event appends.
- No lifecycle mutation.
- No Postgres persistence.
- No API or frontend surface in this issue.

