---
title: TF-F078 Diagnosis - Evidence API Vite Proxy Gap
type: feedback-diagnosis
status: raw
created: 2026-07-13
tags: [TradeForge, TF-F078, feedback, vite, evidence-density, frontend]
source:
  - knowledge/raw/archived/Error-Evidence refresh returned a non-JSON response.md
---

# TF-F078 Diagnosis - Evidence API Vite Proxy Gap

## Source Observation

The operator reported:

```text
Evidence refresh returned a non-JSON response. Check the local dev proxy and
API route configuration.
```

The raw feedback confirmed that `POST /evidence/refresh/run` exists in the
FastAPI Swagger surface. The backend route is current and registered.

## Observable Gap

The frontend dev server returned HTML for an API request that expected JSON.
That indicates Vite handled `/evidence/refresh/run` as a frontend route instead
of proxying it to FastAPI.

## Root Cause

`frontend/vite.config.ts` did not include a proxy entry for the new `/evidence`
prefix introduced by the EV slice. It also did not include `/market`, although
the runtime has `/market/snapshots`.

## Classification

`TF-F078` is a feedback-originated operational bug. It is not an evidence
domain bug and not a backend route-registration failure.

## Invariant Impact

- UX Is Architectural: dev-mode routing must preserve the operator-facing
  workflow rather than failing with opaque HTML responses.
- Derived State Must Remain Distinguishable: the fix does not change evidence
  authority; it only restores access to advisory endpoints.

## Scope Boundary

Minimum correct fix:

- add `/evidence` to the Vite proxy
- add `/market` to the Vite proxy for the existing market API prefix
- add a regression check that static frontend API prefixes are represented in
  the Vite proxy

Out of scope:

- backend route changes
- Event Ledger changes
- lifecycle changes
- API contract changes
- production deployment configuration
