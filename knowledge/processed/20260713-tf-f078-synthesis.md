---
title: TF-F078 Synthesis - Evidence API Vite Proxy Gap
type: processed-synthesis
status: processed
created: 2026-07-13
updated: 2026-07-13
tags: [TradeForge, TF-F078, feedback, vite, frontend, evidence-density]
source_history:
  - knowledge/raw/archived/Error-Evidence refresh returned a non-JSON response.md
  - knowledge/raw/20260713-tf-f078-diagnosis.md
  - knowledge/raw/20260713-tf-f078-planning.md
related:
  - "[[Evidence Density And Attention Ranking]]"
issues:
  - TF-F078
---

# TF-F078 Synthesis - Evidence API Vite Proxy Gap

## What The Feedback Identified

After the EV slice landed, the Evidence Refresh UI action returned a non-JSON
response. The feedback analysis showed that FastAPI had the route registered,
but the Vite dev server returned frontend HTML because it did not proxy the
new `/evidence` API prefix.

## Root Cause

`frontend/vite.config.ts` did not include `/evidence`. The same proxy audit
identified `/market` was also absent despite the existing `/market/snapshots`
route.

## Change Made

The runtime fix adds:

- `/evidence` proxying to `http://127.0.0.1:8000`
- `/market` proxying to `http://127.0.0.1:8000`
- a regression test that checks static frontend API prefixes in
  `frontend/src/api/runtime.ts` are covered by the Vite proxy

## Invariants

Relevant invariant:

- [[UX Is Architectural]]

The change preserves the EV authority boundary. Evidence refresh and ranking
remain advisory/derived; this fix only restores the dev server path that lets
the UI reach FastAPI.

## Replay And Lifecycle Implications

None. No event types, lifecycle transitions, replay reconstruction behavior,
or API contract shapes changed.

## Closure

The original source observation is closed when the dev UI request to
`/evidence/refresh/run` is proxied to FastAPI instead of receiving Vite HTML.
The regression test guards this class of dev proxy omission for static frontend
API prefixes.
