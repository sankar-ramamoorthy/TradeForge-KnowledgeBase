---
title: TF-F077 Diagnosis - ATKR Thesis Import Feedback
type: raw-diagnosis
status: raw
tags: [TradeForge, TF-F077, diagnosis, thesis-import, local-import, verification]
created: 2026-05-28
updated: 2026-05-28
issues:
  - TF-F077
---

# Observable Gap

The operator reported an import preview result of `scanned 2 files; imported 0`
and `No eligible plan draft artifacts for ATKR`, then clarified the intended
artifact was `ATKR_thesis_draft.md`.

# Root Cause

The pasted UI state was the Plan Import Preview. That route intentionally
filters for `plan_draft.v1` artifacts and ignores `thesis_draft.v1` artifacts.

The thesis import route was tested against the live backend and successfully
imported the reported ATKR thesis file:

- `POST /advisory/thesis-imports/scan-local?...symbol=ATKR`
- Result: `scanned_count: 2`, `imported_count: 1`
- `GET /advisory/thesis-imports?...symbol=ATKR`
- Result: `total_count: 1`

# Relevant Invariants

- Advisory import artifacts remain non-canonical.
- Local scan must not append Event Ledger records.
- Lifecycle advancement remains operator-owned.
- The scanner must not infer arbitrary thesis fields from prose.

# Minimum Correct Scope

Close the feedback loop with live verification and preserve the current route
boundary. Do not change parser behavior, thesis schema, mapped fields, event
payloads, lifecycle behavior, or frontend workflow.

# Out Of Scope

- AI parsing from arbitrary prose.
- New thesis fields or schema versions.
- Plan import body mapping.
- Automatic thesis creation.
