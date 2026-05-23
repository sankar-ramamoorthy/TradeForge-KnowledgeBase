---
title: TF-F047 Through TF-F052 Advisory Services And Routes Synthesis
type: processed-synthesis
status: processed
created: 2026-05-22
updated: 2026-05-22
source_history:
  - knowledge/raw/archived/20260522-tf-f047-f052-advisory-services-and-routes-implementation.md
  - knowledge/raw/archived/20260522-m13-completion-summary.md
tags: [TradeForge, M13, advisory, API, LLM, on-demand]
related:
  - "[[AdvisoryArtifact]]"
  - "[[AdvisoryCandidate]]"
  - "[[AdvisoryObservation]]"
  - "[[AdvisoryInterpretation]]"
---

# TF-F047 Through TF-F052 Advisory Services And Routes Synthesis

## Stabilized Outcome

TF-F047 through TF-F052 operationalized on-demand advisory generation through
service boundaries and API routes.

The completed advisory tasks are:

- replay summary
- thesis review
- advisory observation generation
- candidate screening
- advisory provider health check

All generation paths return ephemeral advisory output unless the operator later
accepts and captures the content through the relevant workflow.

## Service Boundary

The services package domain context into `AdvisoryRequest` objects:

- replay timeline context for replay summaries
- structured thesis context for thesis review
- instrument and market context for observation generation
- advisory candidate queue context for candidate screening

The services do not mutate candidates, observations, interpretations, lifecycle
state, or event ledger history.

## API Boundary

The API exposes explicit on-demand routes:

- `POST /advisory/replay-summary`
- `POST /advisory/thesis-review`
- `POST /advisory/generate-observations`
- `POST /advisory/screen-candidates`
- `GET /advisory/health`

Responses include `requires_operator_acceptance: True`.

## Health Semantics

Advisory health distinguishes configured and available provider state from
unavailable or not-configured states. Health check failure should degrade only
advisory generation, never lifecycle, replay, market data, or canonical event
operations.

## Workflow Implication

The stable workflow is:

```text
operator requests advisory generation
    -> advisory response returned
    -> operator reviews caveats and provenance
    -> optional explicit capture or lifecycle action
```

No background or automatic advisory generation is implied by these routes.

## Deferred UX Productization

The API contracts are stable for frontend integration. Workspace trigger buttons
and advisory panels remain UI follow-on work and must preserve advisory labels,
caveats, provenance, and the absence of authoritative action controls.
