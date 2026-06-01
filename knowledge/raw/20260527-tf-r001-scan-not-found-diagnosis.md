---
title: TF-R001 Scan Not Found Diagnosis
type: feedback-diagnosis
status: raw
source_note: knowledge/raw/feed back Local.md thesis import scan failed
context: M14C thesis import workflow manual test
tags:
  - TradeForge
  - TF-R001
  - M14C
  - feedback
  - diagnosis
created: 2026-05-27
---

# TF-R001 Scan Not Found Diagnosis

## Source Observation

The operator attempted the local thesis import scan from the Thesis Development
modal after placing `ATKR_thesis_draft.md` under:

```text
imports/incoming/
```

The UI reported:

```text
Local thesis import scan failed: Not Found
```

## Diagnosis

The current source tree contains the expected route:

```text
POST /advisory/thesis-imports/scan-local
```

The targeted regression test passes in the current checkout:

```text
uv run pytest tests\test_advisory_artifact.py::test_local_thesis_import_scan_persists_markdown_dropoff
```

Directly calling the already-running local API at `127.0.0.1:8000` returns:

```json
{"detail":"Not Found"}
```

That combination indicates operational drift: the frontend is reaching a
running backend process that does not expose the TF-R001 scan route from the
current checkout. The likely cause is a stale or different FastAPI runtime
process, not a failure of the route implementation in the current source tree.

## Invariant Impact

No lifecycle, event, replay, or advisory-boundary invariant is violated by the
diagnosed route behavior in the source tree.

The failure is operational: the operator-facing workflow cannot complete while
the frontend is connected to a backend instance that does not match the current
runtime code.

## Scope Boundary

This diagnosis does not expand TF-R001 semantics.

In scope:

- record the failed manual test feedback
- distinguish stale-runtime drift from route implementation failure
- keep TF-R001 remaining acceptance open until manual retest succeeds against
  the correct backend

Out of scope:

- changing lifecycle semantics
- adding filesystem watchers
- adding automatic thesis creation
- adding new advisory artifact roles
- modifying event taxonomy

