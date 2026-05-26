---
title: "TF-F069 — Provider Governance JSON Parse Failure (Resolved)"
type: processed-note
status: resolved
issue: TF-F069
date: 2026-05-23
raw_source: "raw/TF-F069-provider-governance-json-parse.md"
tags:
  - feedback-issue
  - provider-governance
  - vite-proxy
  - frontend
  - bug
---

# TF-F069 — Provider Governance JSON Parse Failure

## Summary

The Provider Governance workspace rendered only its header and a JSON parse error on load. Root cause: `/provider-governance` was missing from the Vite dev-server proxy table. Vite served the React SPA shell (HTML) in response to the API call; `.json()` then threw.

## Resolution

Added `/provider-governance` and `/admin` to `frontend/vite.config.ts` proxy rules. No backend changes, no ADR, no event model impact.

## Pattern Preserved

Any new backend path prefix that the frontend fetches must be added to the Vite proxy. The existing proxy table (`/health`, `/session`, `/workspaces`, `/lifecycle`, `/replay`) serves as the canonical list — new surfaces must extend it at implementation time.

## Files Changed

- `frontend/vite.config.ts` — added `/provider-governance` and `/admin` proxy entries

## Verification

- Provider Governance workspace loads content (Overview, AI Gateway, Diagnostics, provider list).
- Browser console is free of JSON parse errors on the Provider Governance route.
- Existing provider governance and admin credential tests continue to pass.

## Linked Issue

TF-F069 — Planned → Done
