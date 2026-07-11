---
title: Routes.py Decomposition Plan — API Boundary Refactor
type: implementation-plan
status: raw
tags: [tradeforge, refactor, api-boundary, routes, maintainability, implementation-plan]
created: 2026-07-09
updated: 2026-07-09
---

# Routes.py Decomposition Plan — API Boundary Refactor

Proposed milestone: **M-RF — API Boundary Decomposition**.
Derived from [[20260709-comprehensive-product-audit]] finding F6. Written for
execution by a Codex/Sonnet-class coding agent. This is a *mechanical*
refactor: zero behavior change, zero route change, zero schema change.

Prerequisite reading for the implementing agent: runtime `AGENTS.md`,
`INVARIANTS.md` §9 (layer separation), ADR-0020 (FastAPI runtime boundary).

---

# 1. Verified Current State (inventory, 2026-07-09)

`src/app/api/routes.py` — 7,504 lines, one file containing:

| Section | Lines (approx) | Contents |
|---|---|---|
| Imports | 1–140 | ~35 import groups across all layers |
| Router declarations | 142–149 | 8 routers: `runtime` (no prefix), `lifecycle`, `replay`, `workspaces`, `provenance`, `market`, `advisory`, `behavioral` |
| Pydantic models | 200–1785 | ~150 request/response models |
| Service accessors | 1785–2330 | ~30 `_x_service_from(request)` helpers reading `request.app.state` |
| Response mappers | 2330–3345 | ~35 `_x_response(...)` domain→schema mappers |
| **Markdown import subsystem** | ~2833–3060 | YAML frontmatter parsing, section extraction for local thesis/plan imports — **business logic in the app layer** |
| Route handlers | 3345–7497 | 82 handlers, with ~12 more model classes defined mid-file (e.g. `ThesisArtifactResponse` @3708, interpretation-analytics models @6845–6934) |
| Wiring | 7498–7504 | all sub-routers `include_router`ed into `runtime_router` |

Coupling facts that make this refactor safe:

- Only `application.py` imports from `routes.py` (single symbol:
  `runtime_router`). No test imports `routes.py` directly.
- Tests exercise the app via `create_app()` + `TestClient` — they lock HTTP
  behavior, not module structure. They are the safety net, not an obstacle.
- Provider-governance endpoints (~5307–5630) hang off `runtime_router`
  directly rather than a dedicated router — they get their own module.

# 2. Goals And Non-Goals

Goals:

- No file in `src/app/api/` exceeds ~800 lines.
- One module per API domain; models, mappers, and handlers travel together
  per domain (models are only shared within their domain in practice).
- Shared service accessors become one `deps.py` module.
- The misplaced markdown-import parsing moves to the services layer
  (the only *semantic* move in this plan — isolated in its own phase).

Non-goals (explicitly forbidden to the implementing agent):

- No route path, method, status code, or response shape changes.
- No renaming of model classes, fields, or handler functions.
- No conversion of accessors to FastAPI `Depends()` injection (tempting,
  behavior-adjacent, out of scope — record as a future idea only).
- No async conversions, no error-handling "improvements", no dead-code
  deletion beyond what the move itself makes unreferenced.
- No test edits except import paths (and per the coupling facts, even those
  should be unnecessary).

# 3. Target Structure

```text
src/app/api/
├── application.py          (unchanged wiring; still imports runtime_router)
├── admin_routes.py         (unchanged)
├── deps.py                 (all _x_from(request) service accessors)
├── shared_schemas.py       (only models used by 2+ domains, if any emerge)
├── routes/
│   ├── __init__.py         (builds + exports runtime_router; include_router calls)
│   ├── runtime.py          (health, session)
│   ├── lifecycle.py        (transitions, ideas, thesis, plan, review,
│   │                        scenario branches, annotations, readiness)
│   ├── replay.py           (reconstruction, timeline, cognitive snapshot)
│   ├── workspace.py        (projections, attention queue, playbook summary)
│   ├── market.py           (snapshots, overlays, fundamentals, contextual summary)
│   ├── provenance.py       (market-data provenance)
│   ├── advisory.py         (observations, candidates, artifacts,
│   │                        interpretations, import previews)
│   ├── advisory_generation.py  (LLM-backed: health, replay-summary,
│   │                        thesis-review, observation-gen, screening)
│   ├── advisory_analytics.py   (influence, drift, conflicts, timelines)
│   ├── behavioral.py       (signals, clusters, mistakes, discipline, timelines)
│   └── governance.py       (provider config, governance, gateway,
│                            model selection, smoke tests)
└── routes.py               (DELETED in final phase; interim: thin shim
                             re-exporting runtime_router)
```

Each `routes/<domain>.py` holds, in order: its Pydantic models, its private
mappers, its handlers. `src/app/api/routes/__init__.py` is the only place
that assembles `runtime_router`.

Services-layer addition (Phase 6 only):

```text
src/services/advisory/local_import_parsing.py
    (markdown/frontmatter parsing moved out of the HTTP layer)
```

# 4. The Safety Harness (build FIRST)

## TF-RF001: OpenAPI contract snapshot test

- New test `tests/test_api_contract_snapshot.py`:
  1. Build `create_app()`, call `app.openapi()`.
  2. Serialize deterministically (sorted keys) to
     `tests/snapshots/openapi_contract.json`; commit it.
  3. Test asserts current schema == committed snapshot, with a clear
     failure message ("API contract changed — forbidden during M-RF").
- Also snapshot the route table: sorted list of `(method, path, name)`.
- This is the golden gate. Every subsequent phase must end with this test
  green and **unmodified**. If the implementing agent ever needs to change
  the snapshot during M-RF, it has made a mistake — stop and revert.
- Acceptance: test passes pre-refactor; snapshot file committed.

# 5. Phased Issue Breakdown

Rules for every phase: move-only edits; after each phase run
`uv run pytest && uv run ruff check . && uv run mypy src tests` plus the
snapshot test; commit per phase; never proceed on red.

## TF-RF002: Extract `deps.py`

- Move all ~30 `_x_service_from(request)` accessors (routes.py 1785–2330)
  to `src/app/api/deps.py` unchanged (drop the leading underscore is NOT
  allowed — keep names verbatim; import them where used).
- routes.py imports them back during the transition.
- Lowest-risk phase; proves the loop (move → import → gates green).

## TF-RF003: Create `routes/` package + move `runtime` and `behavioral`

- Create `src/app/api/routes/__init__.py` that, for now, imports the
  monolith's routers and re-exports `runtime_router` (assembly stays at the
  bottom of the old file until TF-RF010).
- Move the two easiest domains end-to-end to establish the pattern:
  - `runtime.py`: health + session handlers and their 4 models.
  - `behavioral.py`: 8 handlers (~6015–6356), their ~20 models
    (403–585), and `_behavioral_source_refs_response`.
- The old routes.py keeps `from .routes.behavioral import behavioral_router`
  etc. so line 7498 wiring still works.
- Acceptance: snapshot green; routes.py shrank by the moved line count;
  no duplicated definitions (grep-verify each moved class name appears
  exactly once under `src/`).

## TF-RF004: Move `replay`, `provenance`, `market`

- `replay.py`: handlers + models 1318–1390 + mappers
  `_replay_projection_response`, `_replay_timeline_response`,
  `_source_linked_artifact_response`, `_historical_*`.
- `provenance.py`: single handler + models 1764–1783.
- `market.py`: snapshots, market-context overlay, fundamentals overlay,
  contextual summary handlers + models 1461–1510 & 1696–1762 + the small
  interpretation-headline helpers (5708–5735).

## TF-RF005: Move `workspace`

- `workspace.py`: projection/attention/playbook handlers + models
  1299–1460 + mappers `_workspace_*`, `_operational_attention_queue_response`,
  `_entity_reference_payloads`, `_default_persona_context`,
  `_workspace_projection_context_from_query`.

## TF-RF006: Move `lifecycle` (largest single domain)

- `lifecycle.py`: all `/lifecycle` handlers (transitions, new idea, list
  decisions, thesis develop/revise/history, plan create/arm/revise/history,
  readiness, review, scenario branches, cognitive snapshot, annotations)
  + models 921–1297 + the mid-file `ThesisArtifactResponse` (3708).
- Watch item: this domain shares `EntityReferencePayload` (230) with
  advisory — if both need it, it goes to `shared_schemas.py` (the first
  legitimate tenant).

## TF-RF007: Move `advisory` family (three modules)

- `advisory.py`: observations, candidates, artifacts, interpretations,
  import-preview/scan handlers + models 230–397, 587–860 + their mappers
  (2377–3140 minus the parsing subsystem, which moves in TF-RF009).
- `advisory_generation.py`: `_ai_advisory_service_from`,
  `_advisory_response_to_model`, health + 4 generation handlers + payload
  models 852–920.
- `advisory_analytics.py`: influence/drift/conflict/timeline handlers +
  the mid-file model block 6845–6934.
- Largest phase; the agent may split into three commits (one per module),
  gates green after each.

## TF-RF008: Move `governance`

- `governance.py`: provider configuration/governance/gateway/model-selection/
  smoke-test handlers (5307–5630) + models 1495–1694 + helpers
  `_credential_status_for`, `_provider_health_status`,
  `_infer_underlying_provider_id`, `_litellm_gateway_metadata`,
  `_build_ai_gateway_response`, `_discover_litellm_models`,
  `_build_advisory_model_selection_response`,
  `_update_advisory_model_selection`,
  `_build_llm_provider_secret_injection_response`.
- These currently sit on `runtime_router`; they must be moved onto a new
  `governance_router = APIRouter(tags=["governance"])` **only if** paths
  can be preserved exactly (they are absolute paths like
  `/provider-governance/...` — verify against the snapshot; if tag changes
  alter the OpenAPI snapshot, keep `tags=["runtime"]`. Path fidelity wins
  over tidiness.)

## TF-RF009: Layer correction — local import parsing to services

- The ONLY semantic move. Relocate the markdown import subsystem
  (`_local_thesis_import_artifact_from_markdown`,
  `_local_plan_import_artifact_from_markdown`,
  `_split_markdown_frontmatter`, `_parse_simple_yaml_frontmatter`,
  `_mapped_fields_from_markdown_sections`,
  `_mapped_plan_fields_from_markdown_sections`, `_section_text`,
  `_section_list`, `_validated_import_field_names`,
  `_local_import_already_persisted` and companions, ~2722–3078) into
  `src/services/advisory/local_import_parsing.py` as public functions.
- Rationale: parsing business documents is orchestration-layer work, not
  HTTP-boundary work (INVARIANTS §9); it also serves the M14C import
  bridge which may later gain non-HTTP callers.
- Existing tests covering import scans (`test_frontend_root_scripts` /
  TF-F077 / TF-R001-adjacent tests) must pass unchanged.
- This phase touches behavior-adjacent code: the agent must diff the moved
  function bodies against the originals byte-for-byte (only import lines
  and `_` prefix removal may differ).

## TF-RF010: Final assembly + delete the monolith

- Move router declarations and `include_router` wiring into
  `routes/__init__.py`; point `application.py` at
  `from src.app.api.routes import runtime_router`; delete `routes.py`.
- Grep-verify zero remaining references to `src.app.api.routes` as a module
  file; snapshot green; full gates green.
- Update `ARCHITECTURE.md`/README repository-structure section; ADR
  checkpoint: no new ADR needed (structure change within ADR-0020's
  boundary), but note the refactor in the ADR-0020 file's history section
  if the project convention supports amendments.

# 6. Sizing And Order Summary

```text
TF-RF001  snapshot harness          ~1h    (do first, never skip)
TF-RF002  deps.py                   ~1h
TF-RF003  runtime + behavioral      ~2h    (pattern-setting)
TF-RF004  replay/provenance/market  ~2h
TF-RF005  workspace                 ~2h
TF-RF006  lifecycle                 ~3h
TF-RF007  advisory ×3               ~4h
TF-RF008  governance                ~2h
TF-RF009  import parsing → services ~2h    (only semantic move; extra care)
TF-RF010  assembly + delete         ~1h
```

Every phase is independently shippable; the refactor can pause indefinitely
after any phase with the codebase fully working. Do NOT interleave M-RF with
feature milestones — land it before or after M-PT, not during (M-PT's
TF-P009 already creates its routes in a separate file, so ordering is
flexible; if M-RF lands first, TF-P009 drops into `routes/execution.py`).

# 7. Agent Guardrails (paste into the implementing session)

```text
1. You are doing a move-only refactor. If you feel the urge to improve
   anything, write it in a NOTES.md and do not do it.
2. The OpenAPI snapshot test is the definition of success. You may never
   edit the snapshot.
3. After every phase: pytest, ruff, mypy, snapshot — all green before the
   next phase.
4. Every moved symbol keeps its exact name. Grep each moved class/function
   after the move: exactly one definition in src/.
5. If a move creates a circular import, the fix is moving the shared model
   to shared_schemas.py — never merging modules back together.
6. Commit per phase with the issue ID in the message.
```

# 8. Registered Follow-Ups

- **M-RF2 — FastAPI `Depends()` conversion** (DECIDED, deferred): replace
  `request.app.state` accessor calls with declared dependency injection.
  Must not start until TF-RF010 lands. Full self-contained plan:
  [[20260709-depends-injection-conversion-plan]].
- **M-RF-FE — frontend API client decomposition**: companion audit and plan
  for `frontend/src/api/runtime.ts`:
  [[20260709-frontend-api-client-refactor-plan]].
