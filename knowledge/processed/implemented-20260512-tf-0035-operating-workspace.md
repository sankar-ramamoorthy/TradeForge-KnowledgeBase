---
title: TF-0035 Operating Workspace Implementation
type: processed
status: processed
created: 2026-05-12
issue: TF-0035
milestone: M8
tags:
  - TradeForge
  - runtime-kb-loop
  - frontend
  - app
  - operating-workspace
  - attention-queue
  - workspace-cognition
---

# TF-0035 Operating Workspace Implementation

## Runtime Outcome

Implemented TF-0035 on runtime branch `feature/tf-0035-operating-workspace`.

Runtime changes:

**Backend:**
- `src/app/api/application.py`: Added `OperationalAttentionQueueReadService` to app state.
- `src/app/api/routes.py`: Added `GET /workspaces/operating/attention` endpoint with `AttentionItemResponse` and `OperationalAttentionQueueResponse` pydantic models. Added `_attention_queue_read_service_from`, `_default_persona_context`, and `_operational_attention_queue_response` helpers. Endpoint registered before `GET /workspaces/{route_id}` to avoid path-match conflict.
- `tests/test_operating_workspace.py`: 5 new tests — empty queue, idea-stage decision attention item, authority boundaries, missing params 422, no event ledger mutation.

**Frontend:**
- `frontend/src/api/runtime.ts`: Added `WorkspaceApiParams`, `WorkspaceProjection`, `WorkspaceProjectionField`, `AttentionItem`, `AttentionItemPriority`, `OperatingAttentionQueue` types and `fetchWorkspaceProjection`, `fetchOperatingAttentionQueue`, `buildWorkspaceQuery` functions.
- `frontend/src/workspaces/OperatingWorkspace.tsx`: New component. Fetches projection and attention queue in parallel with abort cleanup. Renders lifecycle stage context (labeled canonical), ordered attention queue items, and projection metadata. Each item card has category badge, priority badge (with data-priority attribute for CSS), explanation text, lifecycle stage if present, and a navigation link to the appropriate workspace using the shared `buildWorkspaceHref` / `findWorkspaceRoute` helpers.
- `frontend/src/styles.css`: Added CSS for lifecycle context panel, attention queue items with priority-coded left borders (danger/attention/muted), category badges, priority badges, route links, empty state, and authority note section.
- `frontend/src/App.tsx`: Imported `OperatingWorkspace`; renders it for `activeRoute.id === "operating"`, all other routes remain `WorkspaceSurface` placeholders.
- `DOCS/ISSUE_REGISTER.md`: TF-0035 marked Done with implementation summary.

## Semantic Boundary

The Operating Workspace is a derived projection surface. It presents attention queue items (DERIVED authority) and lifecycle stage context (CANONICAL authority). It does not mutate event ledger state, lifecycle transitions, or workspace projection truth. Persona profile interpretation for attention priority adjustment is deferred to post-MVP (default BALANCED profile used for MVP).

## Default Persona Profile Design Decision

`PersonaContext` requires a full `PersonaInterpretationProfile`. The app layer does not yet have a persona registry. For MVP, the API endpoint constructs a default profile (SWING time horizon, BALANCED risk, BALANCED velocity, MULTI_FACTOR signals) from the persona_id/version query params. This is an explicit MVP shortcut documented in the route implementation. Post-MVP persona registry work will replace this default.

## Event Impact

No events were added or changed. The endpoint is purely read-only, consuming existing workspace projection and attention queue services.

## Lifecycle Impact

No lifecycle authority was added. Navigation links route to the appropriate workspace for context-aware action. Lifecycle transitions remain the responsibility of the Plan Review and Active Position workspaces.

## Replay Impact

Attention queue items preserve source event types. Workspace projection response includes source event references. Operating Workspace state is fully reconstructable from the event ledger through existing replay-compatible services.

## Validation

Completed verification:

- `uv run pytest tests/test_operating_workspace.py` — 5 passed
- `uv run pytest` — 183 passed
- `uv run ruff check .` — clean
- `uv run mypy src tests` — clean
- `npm.cmd run typecheck` — clean
- `npm.cmd run lint` — clean
- `npm.cmd run build` — clean (Vite production build)

## Follow-Up Boundary

- TF-0036 (Opportunity Workspace) is responsible for implementing scenario-based opportunity development.
- Persona profile registry (replacing the default MVP profile) is post-MVP work.
- Attention queue route links navigate to placeholder workspace surfaces until TF-0036 through TF-0040 are implemented.
