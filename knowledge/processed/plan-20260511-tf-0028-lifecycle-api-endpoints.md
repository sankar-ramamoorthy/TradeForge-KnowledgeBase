---
title: Plan - TF-0028 Lifecycle API Endpoints
type: processed-planning
status: processed
created: 2026-05-11
source: knowledge/raw/archived/20260511-plan-tf-0028-lifecycle-api-endpoints.md
issue: TF-0028
milestone: M7
branch: feature/tf-0028-lifecycle-api-endpoints
related:
  - [[Decision Lifecycle Engine]]
  - [[LifecycleOrchestrationService]]
  - [[Event Store Port]]
  - [[Event Ledger]]
---

# Plan - TF-0028 Lifecycle API Endpoints

## Stabilized Planning Summary

TF-0028 introduces lifecycle transition HTTP endpoints as app-layer adapters over the existing lifecycle orchestration service. The planning boundary is strict: FastAPI may parse requests and translate outcomes, but lifecycle validation, transition legality, and event appends remain service-layer responsibilities.

## Semantic Observations

- lifecycle transition requests are operational inputs, not lifecycle authority;
- accepted transitions must still become explicit event-backed facts in the [[Event Ledger]];
- invalid transitions must fail explicitly rather than being hidden by route-layer convenience behavior;
- FastAPI remains an adapter boundary and does not redefine [[Decision Lifecycle Engine]] semantics.

## Architecture Boundaries

The implementation should preserve these boundaries:

- FastAPI routes: request parsing and HTTP response translation only.
- [[LifecycleOrchestrationService]]: derives current lifecycle state and validates next-stage requests.
- [[Event Store Port]]: only boundary for appending accepted lifecycle transition events.
- [[Event Ledger]]: canonical truth remains append-only and immutable.

TF-0028 should not add:

- direct Postgres access from routes;
- route-local lifecycle validation rules;
- replay API behavior;
- workspace projection APIs;
- hidden mutable workflow state in HTTP handlers.

## ADR Assessment

No new ADR is required. Runtime ADR 0002 and ADR 0020 already establish the required authority split:

- ADR 0002 keeps lifecycle authority in the lifecycle engine and requires explicit event-backed transitions.
- ADR 0020 keeps FastAPI in the app layer and forbids HTTP handlers from owning workflow or persistence semantics.

## Runtime Implementation Expectations

Runtime implementation should add:

- request and response DTOs for lifecycle transitions;
- a route that delegates transition requests to the injected lifecycle orchestration service;
- explicit HTTP conflict mapping for invalid lifecycle transitions;
- tests covering valid first transition, invalid skipped transition, and event persistence across requests within one app instance;
- runtime issue-register synchronization after successful verification.

## Processing Result

This planning capture stabilizes implementation intent and boundary discipline only. It does not introduce new ontology, workflow, playbook, or doctrine structures.
