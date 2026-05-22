---
title: TF-A008 Through TF-A010 Advisory Observation Closeout Synthesis
type: processed-synthesis
status: processed
created: 2026-05-21
source_history:
  - knowledge/raw/archived/20260521-tf-a008-tf-a010-advisory-observation-closeout.md
tags: [tf-a008, tf-a009, tf-a010, m12, contextual-observation, conflict, staleness]
related:
  - "[[Runtime KB Development Loop]]"
  - "[[AI Advisory Boundary]]"
  - "[[Advisory Observation]]"
---

# TF-A008 Through TF-A010 Advisory Observation Closeout Synthesis

## Stabilized Outcome

TF-A008, TF-A009, and TF-A010 are complete.

M12 advisory observations now support lightweight contextual framing, explicit
conflict visibility, and derived staleness visibility while preserving the
boundary between advisory observation metadata and M13 interpretation semantics.

## Architecture Result

The runtime now has M12 observation-side support for:

- non-canonical contextual observation artifacts
- conflict markers on evidence and caveat-derived unresolved conflict markers
- derived evidence staleness metadata
- API exposure of contextual/conflict/staleness metadata as advisory data

## Boundary Preservation

The implementation explicitly avoids:

- `AdvisoryInterpretation` semantics
- thesis influence
- contextual weighting
- confidence ranges
- recommendation authority
- lifecycle authority
- execution authority
- scoring or ranking

## Runtime Validation

Focused tests passed:

```text
uv run pytest tests\test_advisory_observation.py tests\test_replay_timeline.py tests\test_replay_timeline_service.py tests\test_advisory_interpretation.py
```

Result:

```text
34 passed, 1 warning
```

Focused ruff checks passed on touched non-API files and the new migration.

## Follow-On Issue

The next issue is TF-A011: Implement advisory candidate ingestion pipeline.

TF-A011 should keep candidate ingestion in advisory space and must not create
TradeIdea lifecycle events, thesis artifacts, plans, approvals, or execution
intent.

