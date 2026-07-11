---
title: FastAPI Depends() Conversion Plan — API Dependency Injection Modernization
type: implementation-plan
status: raw
tags: [tradeforge, refactor, fastapi, dependency-injection, api-boundary, implementation-plan]
created: 2026-07-09
updated: 2026-07-09
---

# FastAPI Depends() Conversion Plan — API Dependency Injection Modernization

Proposed milestone: **M-RF2 — API Dependency Injection**.
Registered as deferred follow-up in
[[20260709-routes-refactor-implementation-plan]] (M-RF).

**Hard prerequisite: M-RF (TF-RF001–TF-RF010) must be fully landed.** Do not
start this milestone if `src/app/api/routes.py` still exists as a monolith.
Verify first: `src/app/api/routes/` package exists, `src/app/api/deps.py`
exists, and `tests/snapshots/openapi_contract.json` is committed and green.

This document is self-contained so a coding agent with no prior session
context can execute it. The agent should still follow runtime `AGENTS.md`
discipline (issue registration first) and read `INVARIANTS.md` §9.

---

# 1. Background: What Is Being Changed And Why

## Current pattern (service locator)

`create_app()` in `src/app/api/application.py` constructs every service and
attaches it to application state (`app.state.lifecycle_service = ...`).
Route handlers receive the raw `Request` and fetch services through accessor
functions that after M-RF live in `src/app/api/deps.py`:

```python
# deps.py (post-M-RF, names preserved verbatim from the monolith)
def _lifecycle_service_from(request: Request) -> LifecycleOrchestrationService:
    return request.app.state.lifecycle_service

# routes/lifecycle.py
@lifecycle_router.post("/transitions", response_model=LifecycleTransitionResponse)
def create_lifecycle_transition(
    request: Request, payload: LifecycleTransitionPayload
) -> LifecycleTransitionResponse:
    service = _lifecycle_service_from(request)
    ...
```

## Target pattern (FastAPI dependency injection)

```python
# deps.py
def get_lifecycle_service(request: Request) -> LifecycleOrchestrationService:
    return request.app.state.lifecycle_service

# routes/lifecycle.py
@lifecycle_router.post("/transitions", response_model=LifecycleTransitionResponse)
def create_lifecycle_transition(
    payload: LifecycleTransitionPayload,
    service: Annotated[LifecycleOrchestrationService, Depends(get_lifecycle_service)],
) -> LifecycleTransitionResponse:
    ...
```

## Why

- Handlers declare dependencies in their signature; acquisition code
  disappears from bodies.
- Tests gain `app.dependency_overrides[get_lifecycle_service] = lambda: fake`
  — swapping one service without threading it through `create_app()`
  keyword arguments.
- Future work composes cleanly: an operator-identity dependency (family
  multi-user) or an execution-service guard (paper trading) becomes a
  declared dependency other dependencies can require.

## Critical invariant of this conversion

A dependency function whose only parameter is `request: Request` adds
**nothing** to the OpenAPI schema. Therefore the committed snapshot
(`tests/snapshots/openapi_contract.json`) must remain byte-identical through
this entire milestone — same golden gate as M-RF. If the snapshot changes,
a dependency function has declared a query/body/path parameter by mistake.

# 2. Scope Rules

In scope:

- Accessor functions in `deps.py` whose sole parameter is
  `request: Request` and whose body is a one-line read of
  `request.app.state.<attr>` (approximately 30 functions).
- Handler signatures across all modules in `src/app/api/routes/` and
  `src/app/api/admin_routes.py` (if it uses the same pattern — verify).
- Helper functions that take `request` solely to reach services
  (Phase C below).

Out of scope (forbidden):

- Any change to `create_app()` construction/wiring order or its optional
  keyword parameters (test plumbing simplification is a later, separate
  decision).
- Any change to routes, response models, status codes, error handling.
- Rewrites of existing tests (they must pass unmodified; import-line
  changes only if an accessor rename requires it — see Phase A).
- Converting `app.state` storage itself to some other container.
- Async conversions, lifespan changes, middleware.

# 3. Preliminary Inventory (agent's first task)

Before editing, produce and commit a scratch inventory
(`NOTES-m-rf2-inventory.md`, deleted at closeout) listing:

1. Every function in `deps.py` matching the accessor shape
   (`grep -n "def .*(request" src/app/api/deps.py`), and for each: the
   `app.state` attribute read, and every call site
   (`grep -rn "<name>(" src/app/api/`).
2. Call sites split into two classes:
   - **Class H**: called directly inside a route handler body.
   - **Class F**: called inside a non-handler helper (e.g. the governance
     helpers `_build_ai_gateway_response(request)`,
     `_build_advisory_model_selection_response`,
     `_discover_litellm_models` pattern — helpers that take `request` and
     internally call accessors).
3. Any accessor with parameters beyond `request` or logic beyond a state
   read — these are NOT accessors; list them as excluded.

Also verify whether tests import any accessor by name
(`grep -rn "deps import\|_from(" tests/`); expected answer per M-RF: none.

# 4. Phased Issue Breakdown

Gates after every phase:
`uv run pytest && uv run ruff check . && uv run mypy src tests`
plus the untouched snapshot test. Commit per phase.

## TF-RF2-001: Register issues + inventory

- Add TF-RF2-001…TF-RF2-006 to `DOCS/ISSUE_REGISTER.md` (milestone M-RF2,
  affected layer: app only, linked ADR: ADR-0020, impacted invariants:
  Layer Separation §9 — unchanged semantics).
- Produce the inventory above.

## TF-RF2-002: Rename accessors to public `get_*` names

- In `deps.py`: `_lifecycle_service_from` → `get_lifecycle_service`, etc.
  (pattern: `_<x>_service_from` → `get_<x>_service`; keep any accessor
  whose name doesn't fit the pattern recognizable, e.g.
  `_event_store_from` → `get_event_store`,
  `_session_provider_from` → `get_session_provider`,
  `_credential_store_from_state` → `get_credential_store`).
- Mechanical sweep of all call sites in `src/app/api/` (imports + calls).
- Grep gate: zero occurrences of the old names anywhere in `src/` and
  `tests/`; each new name defined exactly once.
- Why a separate phase: renames are the highest-typo-risk step; isolating
  them means any failure bisects instantly.

## TF-RF2-003: Convert Class H call sites (handler-direct), one module per commit

For each module in `src/app/api/routes/` (order: behavioral, provenance,
replay, market, workspace, runtime, lifecycle, imports/advisory,
advisory_generation, advisory_analytics, governance):

- For every handler that calls `get_x(request)` directly in its body:
  1. Add a signature parameter
     `x: Annotated[XType, Depends(get_x)]` (import `Annotated` from
     `typing`, `Depends` from `fastapi`). Match the variable name already
     used in the body so the body diff is minimal.
  2. Delete the acquisition line.
  3. Remove `request: Request` from the signature **only if** nothing else
     in the body uses `request`. If anything still uses it, keep it.
- Dependency parameters must come after path/query/body parameters if
  default-less ordering complains; using `Annotated` form avoids default
  ordering issues entirely — prefer it everywhere.
- Per-module gates; snapshot must not change. If the snapshot changes,
  the agent added a non-Request parameter to a dependency — revert and
  re-read this plan.

## TF-RF2-004: Convert Class F call sites (helpers that take request)

- For each helper that takes `request` only to call accessors internally:
  change its signature to accept the needed service objects/values as
  explicit parameters; the calling handler now declares those dependencies
  via `Depends` and passes them down.
- Example shape (governance module has the main cluster):

```python
# before
def _build_ai_gateway_response(request: Request) -> ProviderGovernanceAIGatewayResponse:
    store = get_credential_store(request)
    ...

# after
def _build_ai_gateway_response(
    credential_store: CredentialStore, ...
) -> ProviderGovernanceAIGatewayResponse:
    ...
```

- If a helper needs 4+ services, it may keep taking `request` — note it in
  the inventory as intentionally unconverted rather than forcing an ugly
  signature. Pragmatism over purity; zero behavior change remains the rule.

## TF-RF2-005: Demonstration override test

- Add ONE new test, `tests/test_dependency_overrides.py`: build the app,
  override one dependency (suggest `get_lifecycle_service`) with a stub via
  `app.dependency_overrides`, hit the corresponding endpoint with
  `TestClient`, assert the stub was used, then clear overrides.
- Purpose: proves the milestone's payoff mechanically and documents the
  pattern for future test authors. Do not retrofit existing tests.

## TF-RF2-006: Closeout

- Delete the inventory notes file; final full-gate run; grep gates:
  - `grep -rn "_from(request)" src/app/api/` → zero (or only the
    intentionally-unconverted list from TF-RF2-004)
  - snapshot unchanged since milestone start (`git diff` on the snapshot
    file across the milestone's commits → empty)
- KB synthesis note; ISSUE_REGISTER statuses to Done.
- Record (do not implement) the further deferred idea: simplifying
  `create_app()`'s optional keyword-argument test plumbing now that
  `dependency_overrides` exists.

# 5. Sizing

```text
TF-RF2-001  register + inventory     ~1h
TF-RF2-002  renames                  ~1h
TF-RF2-003  handler conversion       ~3h  (82 handlers, mechanical)
TF-RF2-004  helper conversion        ~1.5h (mostly governance)
TF-RF2-005  override test            ~0.5h
TF-RF2-006  closeout                 ~0.5h
```

# 6. Agent Guardrails (paste into the implementing session)

```text
1. The OpenAPI snapshot may never change in this milestone. A dependency
   function takes exactly one parameter: request: Request. Nothing else.
2. Zero behavior change. No error-handling edits, no logic edits, no
   "while I'm here" improvements — those go to a NOTES.md.
3. Existing tests pass unmodified. You may add exactly one new test file
   (TF-RF2-005).
4. Prefer Annotated[Type, Depends(fn)] over default-value style.
5. Keep request: Request in any handler that still uses it for anything
   other than service acquisition. When in doubt, keep it.
6. One module per commit in TF-RF2-003; issue ID in every commit message.
7. If pytest/mypy/ruff/snapshot is red, fix or revert before proceeding.
   Never stack a new phase on a red baseline.
```

# 7. Relationship To Other Planned Work

- [[20260709-paper-trading-implementation-plan]] (M-PT): TF-P006's rule
  that no advisory code path may reach the execution service becomes
  easier to enforce and test once services are declared dependencies;
  ideally land M-RF2 before M-PT Phase 3, but it is not a blocker.
- Family operator identity (RAMP-03 in
  [[20260709-product-viability-and-ease-of-use-roadmap]]): a future
  `get_current_operator` dependency slots directly into this pattern.
