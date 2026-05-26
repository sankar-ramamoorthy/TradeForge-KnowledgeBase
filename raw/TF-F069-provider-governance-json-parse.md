---
title: "TF-F069 Raw Diagnosis — Provider Governance JSON Parse Failure"
type: raw-capture
status: captured
issue: TF-F069
date: 2026-05-23
tags:
  - feedback-issue
  - provider-governance
  - vite-proxy
  - frontend
  - bug
---

# TF-F069 — Raw Diagnosis Capture

## Observable Gap

Opening the Provider Governance workspace shows only three lines:

```
External Systems
Provider Governance
JSON.parse: unexpected character at line 1 column 1 of the JSON data
```

No governance content loads. The `ProviderGovernanceWorkspace` component enters its error state.

## Diagnosis Trace

### 1. Frontend call site

`frontend/src/api/runtime.ts:346`:

```ts
const response = await fetch("/provider-governance", { signal });
```

The frontend calls `/provider-governance` directly. During development, Vite proxies API calls to the backend at `http://127.0.0.1:8000`.

### 2. Vite proxy configuration (before fix)

`frontend/vite.config.ts`:

```ts
proxy: {
  "/health": "http://127.0.0.1:8000",
  "/session": "http://127.0.0.1:8000",
  "/workspaces": { ... },
  "/lifecycle": "http://127.0.0.1:8000",
  "/replay": "http://127.0.0.1:8000"
}
```

`/provider-governance` is absent. Vite therefore does not proxy the request to the backend — it falls through and serves `index.html` (the React SPA shell).

### 3. Result

The browser receives HTML where it expects JSON. `response.ok` is `true` (200), so the `!response.ok` guard does not fire. Calling `.json()` on the HTML body throws `JSON.parse: unexpected character at line 1 column 1`.

The error propagates to the `.catch()` handler in `ProviderGovernanceWorkspace`, which sets the `error` state and renders the `runtime-error` div — hence the three visible lines.

### 4. Backend state

The backend route is correctly defined:

- `src/app/api/routes.py:4100` — `@runtime_router.get("/provider-governance", ...)`
- `src/app/api/application.py:501` — `app.include_router(runtime_router)` (no prefix)

The backend is not at fault.

### 5. Secondary gap found

`/admin/credentials` (`frontend/src/api/runtime.ts:441`) is also missing from the Vite proxy. The credentials panel would fail with the same JSON parse error under dev.

## Root Cause

`/provider-governance` (and `/admin`) were never added to `vite.config.ts` when TF-F067 implemented the Provider Governance frontend surface.

## Scope of Fix

Minimal: add two proxy entries to `vite.config.ts`.

No backend changes required.
No ADR required (non-architectural — dev tooling configuration only).
No lifecycle, event model, or domain model impact.

## Invariants Touched

- `UX Is Architectural` — broken governance surface degrades operator awareness of external system health.

## Resolution

Added to `frontend/vite.config.ts`:

```ts
"/provider-governance": "http://127.0.0.1:8000",
"/admin": "http://127.0.0.1:8000"
```
