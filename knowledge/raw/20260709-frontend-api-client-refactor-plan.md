---
title: Frontend API Client Audit And Decomposition Plan — runtime.ts
type: implementation-plan
status: raw
tags: [tradeforge, refactor, frontend, api-client, maintainability, implementation-plan]
created: 2026-07-09
updated: 2026-07-09
---

# Frontend API Client Audit And Decomposition Plan — runtime.ts

Proposed milestone: **M-RF-FE — Frontend API Client Decomposition**.
Companion to [[20260709-routes-refactor-implementation-plan]] (backend M-RF);
answers the open question recorded there. Written for a Codex/Sonnet-class
implementer. Analysis date: 2026-07-09.

---

# 1. Audit Findings (verified)

`frontend/src/api/runtime.ts` — 1,856 lines, the sole API module:

- ~95 exported types + ~55 fetch functions covering every backend domain
  (lifecycle, thesis/plan authoring, replay, workspace, market, advisory,
  imports, behavioral, governance/credentials), ordered by accretion date
  rather than domain.
- **33 files** import from it — nearly every workspace component. Import
  stability is the critical constraint.

## F-FE1. Two-class error handling (real defect risk, not just style)

A well-built helper `readOperationalJson<T>` exists (content-type check,
malformed-JSON detection, FastAPI `detail` extraction) — but only **18 of
~67 call sites** use it. The other ~49 hand-roll response handling, many in
the older style of `fetchRuntimeStatus`: no content-type check, `response.json()`
without parse guard, and error text that omits the backend's `detail`. In
practice: newer surfaces show useful errors ("Thesis review failed:
<detail>"), older ones show "request failed: 500". The dev-proxy failure
mode the helper specifically diagnoses (HTML instead of JSON) is unguarded
at those 49 sites — and that failure mode has already produced a real issue
(TF-F069, JSON parse failure on governance load).

## F-FE2. Hand-written type duplicates of backend Pydantic models

Every response type is manually mirrored. There is no drift detection: a
backend field rename typechecks fine on the frontend and fails silently at
runtime (undefined rendering). Backend M-RF's committed OpenAPI snapshot
(`tests/snapshots/openapi_contract.json`) is the natural future source for
generated types — recorded below as deferred work, not in-scope.

## F-FE3. No test runner

Frontend gates are `tsc --noEmit` and `vite build` only ("lint" is an alias
for typecheck). The refactor harness must therefore be compile-time-based.

## F-FE4. Scale verdict

At 1,856 lines this is a *medium* problem — one milestone-sized cleanup,
not an emergency. But every new feature (paper trading M-PT will add an
`execution` client; evidence work will add more) makes it worse, and a
lower-capability model editing a 33-importer monolith risks collateral
damage. Do it once, before M-PT frontend work.

# 2. Goals And Non-Goals

Goals:

- One module per API domain, mirroring backend `routes/` names 1:1 so
  future agents can navigate by symmetry.
- All 33 importers keep working **unchanged** during decomposition
  (barrel re-export = the compile-time safety harness).
- One HTTP helper used by 100% of call sites (the only semantic phase).

Non-goals (forbidden):

- No endpoint path/param changes; no type renames; no field changes.
- No state-management/react-query adoption; no retry/caching logic.
- No OpenAPI codegen in this milestone (deferred issue).
- No edits to workspace components except — in the final phase only —
  optional import-path updates.

# 3. Target Structure

```text
frontend/src/api/
├── http.ts            (requestJson<T> — the single fetch helper)
├── runtime.ts         (becomes a pure barrel: export * from each module;
│                       importers never notice; optionally renamed index.ts
│                       in the final phase with a codemod-style import sweep)
├── lifecycle.ts       (transitions, ideas, decisions, thesis, plan,
│                       readiness, review, scenario branches, annotations,
│                       cognitive snapshot)
├── replay.ts          (timeline)
├── workspace.ts       (projections, attention queue, playbook summary)
├── market.ts          (market context, fundamentals, contextual summary)
├── advisory.ts        (interpretations, candidates, review queue)
├── advisoryGeneration.ts  (thesis review, smoke tests, generated responses)
├── imports.ts         (thesis/plan import previews + local scans)
├── behavioral.ts      (signals, clusters, mistakes, reflections,
│                       timelines, quality metrics)
└── governance.ts      (provider config/governance/gateway, model selection,
                        secret injection, credentials + PROVIDER_CREDENTIAL_SCHEMAS)
```

Domain modules own their types and fetchers together (same rule as backend
M-RF). Cross-domain types (`EntityReferencePayload` is the known candidate)
go to a `shared.ts` only when actually imported by 2+ modules.

# 4. Safety Harness

TypeScript strict mode + the barrel gives the harness for free:

1. `runtime.ts` becomes `export * from "./lifecycle"; …` — every moved
   symbol must still be exported or `npm run typecheck` fails at the
   consumer sites. Missing, renamed, or duplicated symbols cannot slip
   through compilation.
2. Grep gate per phase: each moved symbol defined exactly once under
   `src/api/`.
3. `npm run typecheck && npm run build` green after every phase.
4. Manual smoke per phase (no test runner): load the app, exercise one
   surface from the moved domain (e.g. after behavioral.ts, open the
   behavioral panels).

# 5. Phased Issue Breakdown

## TF-RFE001: Extract `http.ts`

- Move `readOperationalJson` into `http.ts` and wrap it in a general
  helper: `requestJson<T>(path, { method, body, signal, label })` that
  builds the fetch call and applies the helper.
- Move-only for the 18 existing helper call sites (switch them to
  `requestJson` where the mechanical translation is exact; otherwise leave
  them calling the moved `readOperationalJson` directly).
- Do NOT touch the 49 hand-rolled sites yet.
- Acceptance: typecheck + build green; zero behavior change.

## TF-RFE002: Move `behavioral.ts` + `replay.ts` (pattern-setting)

- Smallest clean domains. Move types + fetchers + `buildBehavioralQuery`;
  add `export *` lines to the barrel.
- Establishes the loop: move → barrel → typecheck → build → smoke.

## TF-RFE003: Move `workspace.ts` + `market.ts`

- Includes `buildWorkspaceQuery`, workspace/attention/playbook types,
  market context, fundamentals, contextual summary.

## TF-RFE004: Move `lifecycle.ts` (largest)

- All authoring flows: new idea, decisions list, thesis develop/revise/
  artifact, plan create/arm/revise/artifact, readiness, review, scenario
  branches, cognitive snapshot, annotations.
- `EntityReferencePayload` decision point: if advisory also needs it,
  create `shared.ts` now.

## TF-RFE005: Move `advisory.ts`, `advisoryGeneration.ts`, `imports.ts`

- Interpretations, candidates, review queue; generation + smoke test;
  thesis/plan import previews and local scans.

## TF-RFE006: Move `governance.ts`

- Provider configuration/governance/gateway/model selection/secret
  injection, credentials CRUD, `PROVIDER_CREDENTIAL_SCHEMAS` constant.
- After this phase `runtime.ts` contains only the barrel + runtime
  status/session fetchers (which stay: `runtime.ts` is a truthful name
  for them).

## TF-RFE007: Error-handling unification (the only semantic phase)

- Convert the ~49 hand-rolled call sites to `requestJson`.
- This is a deliberate, reviewed behavior change: error messages become
  richer and non-JSON responses fail with the diagnostic message instead
  of a JSON parse exception. UI code that string-matches error text (grep
  for `.message` comparisons in workspaces first — record findings before
  changing) must be checked.
- Per-domain commits; manual smoke of each surface's failure path where
  practical (stop backend, observe error rendering on one panel per domain).
- Acceptance: zero raw `fetch(` calls left in `src/api/` outside `http.ts`
  (grep gate); typecheck + build green.

## TF-RFE008: Closeout

- Decide barrel end-state: keep `runtime.ts` as barrel (recommended — zero
  churn in 33 files) or rename to `index.ts` and sweep imports.
- Update `frontend/DESIGN.md` with the module map; KB synthesis note.

# 6. Sizing And Sequencing

```text
TF-RFE001  http helper            ~1h
TF-RFE002  behavioral + replay    ~1h
TF-RFE003  workspace + market     ~1.5h
TF-RFE004  lifecycle              ~2h
TF-RFE005  advisory ×3            ~2h
TF-RFE006  governance             ~1.5h
TF-RFE007  error unification      ~2h   (semantic; extra review)
TF-RFE008  closeout               ~0.5h
```

Independent of backend M-RF — can land before, after, or interleaved
(different repos of concern within the runtime repo; no shared files).
Land BOTH before M-PT frontend work so `execution.ts` (client) and
`routes/execution.py` (server) are born into clean structures.

# 7. Agent Guardrails (paste into the implementing session)

```text
1. Move-only until TF-RFE007. Improvements go to NOTES.md, not code.
2. The barrel (runtime.ts export *) must keep every existing symbol
   importable from "../api/runtime" at all times. Never edit workspace
   components to fix a missing export — fix the barrel.
3. After every phase: npm run typecheck && npm run build, plus grep:
   every moved symbol defined exactly once under src/api/.
4. TF-RFE007 only: before changing a call site, grep workspaces for
   string-matching against that fetcher's error messages.
5. Commit per phase with the issue ID.
```

# 8. Deferred (register as future issues, do not implement)

- **OpenAPI-generated types**: generate TS types from backend M-RF's
  committed `openapi_contract.json` to eliminate F-FE2 drift permanently.
  Natural follow-up once both refactors land.
- **Frontend test runner** (vitest): would upgrade the harness from
  compile-time to behavioral; justified when the first real client-side
  logic (not just fetch wrappers) appears — likely M-PT's order-status
  polling UI.
