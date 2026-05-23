# TF-F047 through TF-F052 — Advisory Services and Routes Implementation Note

## Date
2026-05-22

## Issues Covered
- TF-F047: Replay summary prompt template and endpoint
- TF-F048: Thesis review assistant prompt template and endpoint
- TF-F049: Advisory observation generation prompt template and endpoint
- TF-F050: Candidate screening prompt template and endpoint
- TF-F051: On-demand advisory API endpoints and frontend trigger models
- TF-F052: Advisory health check endpoint

## What Was Built

### New Service Files

`src/services/advisory/thesis_review.py` — `ThesisReviewAdvisoryService`
Packages `ThesisArtifact` + symbol into `AdvisoryRequest` with
`THESIS_REVIEW` artifact kind. Serializes narrative, catalysts, assumptions,
invalidation conditions, confidence, and regime alignment.

`src/services/advisory/observation_gen.py` — `ObservationGenerationAdvisoryService`
Packages ticker + instrument_kind + market context summary + optional
fundamentals + regime into `OBSERVATION_GENERATION` request. Generates
candidate observations for operator review.

`src/services/advisory/candidate_screening.py` — `CandidateScreeningAdvisoryService`
Packages `AdvisoryCandidate` queue into `CANDIDATE_SCREENING` request.
Returns advisory commentary without modifying any candidate records.

### New API Endpoints (in routes.py)

`GET /advisory/health`
Returns `available` / `unavailable` / `not_configured`.
For `OpenAICompatibleAdvisoryProvider`, pings the models endpoint as a
lightweight health probe (no token consumption).

`POST /advisory/replay-summary`
Accepts `decision_id`, `persona_id`, `workspace_id`, `operator_question`.
Uses `ReplayAdvisoryService.summarize_timeline()` (already existed).
Returns `AdvisoryGeneratedResponse` — ephemeral advisory content.

`POST /advisory/thesis-review`
Accepts `decision_id` (extracts latest thesis from event history),
`persona_id`, `workspace_id`, `operator_question`.
Returns advisory review of thesis blind spots and assumptions.

`POST /advisory/generate-observations`
Accepts `symbol`, `instrument_kind`, `market_context_summary`,
optional `fundamentals_summary`, `regime_label`, `persona_id`,
`workspace_id`, `operator_question`, optional `decision_id`.
Returns candidate observations for operator acceptance.

`POST /advisory/screen-candidates`
Accepts `persona_id`, `workspace_id`, `operator_question`.
Reads current candidate queue from `CandidateReviewQueueService`.
Returns advisory screening commentary.

### New Pydantic Models (in routes.py)
- `AdvisoryProvenanceResponse`
- `AdvisoryGeneratedResponse` (generic on-demand advisory response)
- `AdvisoryHealthResponse`
- `ReplaySummaryRequestPayload`
- `ThesisReviewRequestPayload`
- `ObservationGenerationRequestPayload`
- `CandidateScreeningRequestPayload`

### Helper Functions (in routes.py)
- `_ai_advisory_service_from(request)` — extracts and wraps provider from
  app state, raises 503 if not configured
- `_advisory_response_to_model(response)` — converts domain `AdvisoryResponse`
  to `AdvisoryGeneratedResponse` Pydantic model

## Architectural Notes

All four endpoints return `requires_operator_acceptance: True` in the response.
This enforces the ADR-0042 invariant: generated advisory content must not be
persisted without operator acceptance. None of these endpoints write to any
store.

`GET /advisory/health` catches all exceptions from the provider ping and
returns `unavailable` rather than propagating a 500. Health check failure
does not affect lifecycle, replay, or market data operations.

The thesis review endpoint reconstructs the thesis from the event store rather
than a separate thesis service, preserving the event-sourced architecture.

## Invariants Preserved
- AI Advisory Boundary (ADR-0006): all outputs are advisory, non-canonical
- Human Decision Sovereignty: operator acceptance required before any capture
- No lifecycle mutations from any advisory endpoint

## Frontend Note (TF-F051)
The frontend trigger surfaces (buttons in workspaces) are documented here
for M13 but the actual React components are part of the TF-B011/B012 UX
issues which cover interpretation-first operational surfaces. The API
contracts are now complete and stable for frontend integration.
