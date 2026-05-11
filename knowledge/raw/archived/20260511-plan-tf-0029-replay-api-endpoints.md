---
title: TF-0029 Replay API Endpoints Planning Capture
type: raw-note
status: archived
created: 2026-05-11
issue: TF-0029
milestone: M7
branch: feature/tf-0029-replay-api-endpoints
---

# TF-0029 Replay API Endpoints Planning Capture

## Issue Frame

- issue: `TF-0029`
- milestone: `M7`
- title: `Add replay API endpoints`
- affected layer: `app, services`
- linked ADRs: `ADR 0008`, `ADR 0014`, `ADR 0020`
- impacted invariants: replay, historical integrity, event sourcing, layer separation

## Planning Notes

Replay API behavior should expose existing replay-derived read models without creating new replay semantics in the HTTP layer.

The route boundary should remain thin:

- `GET /replay` should expose historical reconstruction output;
- `GET /replay/timeline` should expose deterministic replay timeline output;
- both routes should consume existing services/pipelines only;
- neither route should call live APIs, mutate lifecycle state, or own persistence logic.

## Shared Runtime State Requirement

The app runtime should use a shared event-store-backed service set so:

- lifecycle endpoints append canonical events;
- replay endpoints read the same event history within the same app instance;
- replay output remains a deterministic reflection of the current Event Ledger history visible to that runtime.

## ADR Checkpoint

No new ADR appears necessary.

- ADR 0008 already defines deterministic replay reconstruction from the Event Ledger.
- ADR 0014 already defines replay as a first-class derived interaction model.
- ADR 0020 already defines FastAPI as an adapter boundary and scopes replay APIs to TF-0029.

## Expected Runtime Changes

- app factory wiring for shared default event-store-backed lifecycle and replay services;
- replay response DTOs for historical reconstruction and replay timeline outputs;
- HTTP routes that translate service outputs only;
- tests for replay reconstruction, replay timeline, shared runtime event history, and deterministic repeated reads;
- runtime and KB state synchronization after verification.
