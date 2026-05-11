---
title: TF-0027 FastAPI Runtime Planning
type: raw-capture
status: raw
created: 2026-05-11
issue: TF-0027
milestone: M7
branch: feature/tf-0027-fastapi-runtime
related:
  - Runtime KB Development Loop
  - ADR 0020
---

# TF-0027 FastAPI Runtime Planning

## Issue Context

TF-0027 adds FastAPI as the HTTP application boundary.

Issue metadata:
- Issue ID: TF-0027
- Milestone: M7
- Title: Add FastAPI Application Runtime
- Affected layer: app
- Linked ADR: ADR 0020
- Impacted invariants: Layer Separation, Decision Lifecycle

## Planning Summary

TF-0027 should establish the FastAPI runtime shell without implementing lifecycle, replay, or workspace projection endpoints. Those are scoped to TF-0028 through TF-0030.

## Architecture Reasoning

The HTTP layer is an application boundary. It can expose runtime status and wire routers, but it must not own domain rules, lifecycle transitions, event semantics, replay behavior, or persistence semantics.

The app factory should make the application start locally and provide a route registration surface for later API issues. A minimal health/metadata route can verify runtime startup without becoming a domain endpoint.

## ADR Decision

ADR required: yes.

Reason: TF-0027 links ADR 0020, and FastAPI introduces the HTTP application boundary. The runtime repository has the ADR roadmap entry but no materialized ADR 0020 file.

Proposed ADR: `DOCS/adr/0020-fastapi-runtime-boundary.md`.

## Event And Replay Implications

No new events are introduced. No event store behavior changes. Replay behavior is unchanged. The API runtime must not read live external APIs or expose mutable state as truth.

## Lifecycle Implications

No lifecycle transitions are implemented. The HTTP layer must not validate or advance lifecycle stages directly. Later lifecycle endpoints must delegate to lifecycle services.

## Implementation Boundaries

In scope:
- Add FastAPI runtime dependencies and lock update.
- Add app factory and base API router.
- Add health/runtime metadata endpoint that returns non-domain operational status.
- Add local run command/documentation.
- Add tests proving startup and HTTP-layer boundary behavior.
- Materialize ADR 0020.
- Update issue register and roadmap after verification.

Out of scope:
- Lifecycle API endpoints.
- Replay API endpoints.
- Workspace projection APIs.
- Frontend implementation.
- Authentication/session model.

## Open Questions

No semantic blocker identified. Use FastAPI TestClient for tests; this may require httpx through FastAPI/Starlette dependencies.
