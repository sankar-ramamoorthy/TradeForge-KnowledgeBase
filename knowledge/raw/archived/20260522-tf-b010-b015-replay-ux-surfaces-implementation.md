# TF-B010 through TF-B015 — Replay and UX Surfaces Implementation Note

## Date
2026-05-22

## Issues

### TF-B010: Evidence impact replay overlays
Already implemented via M12 infrastructure. `EventDomain.ADVISORY` events
map to `ReplayTimelineEntryKind.ADVISORY` in `ReplayTimelineBuilder`.
`advisory.interpretation_captured` events appear in the replay timeline
with interpretation metadata (id, observation_ids, kind, influence, weight,
confidence, capture_origin, provenance) but NOT content/rationale (per ADR-0042).
Verified in existing test: `test_replay_timeline_includes_interpretation_capture_fact_without_content`.
Additional tests added to confirm overlay metadata is complete.

### TF-B011: Interpretation-first operational surfaces
The existing advisory interpretation API already returns all required fields:
- `content` (the interpretation narrative)
- `caveats` (required, non-empty)
- `provenance_summary` (required)
- `confidence_range` (qualitative, per ADR-0042)
- `thesis_influence` (qualitative classification)
- `contextual_weight` (qualitative weight)
- `authority: "advisory"` label
- `is_canonical: false` label
All interpretation responses are correctly labelled advisory and non-canonical.
The API surfaces are interpretation-first by construction.
No autonomous approve/execute controls exist — only create/read endpoints.

### TF-B012: Uncertainty-preserving UX patterns
Backend enforces: caveats required on AdvisoryInterpretation, uncertainty band
required on AdvisoryObservation, confidence required on AdvisoryUncertainty.
No numeric score fields exist — all representations are qualitative enums.
`AdvisoryGeneratedResponse` includes `caveats`, `confidence`, and `requires_operator_acceptance`.
Frontend note: React components should render confidence_range/uncertainty_band
visually (badge/chip pattern) and caveats in a collapsible list.

### TF-B013: Probabilistic cognition summaries
New `probabilistic_cognition_summary()` method on `AdvisoryInterpretationQueryService`
combines thesis_influence, contextual_weight, confidence_range distributions
into a single advisory read model. New `GET /advisory/cognition-summary` endpoint.

### TF-B014: Evidence narrative generation
`InterpretationDraftService` and `POST /advisory/interpretations/draft` exist
from M12. `OpenAICompatibleAdvisoryProvider` now provides the concrete LLM
backend. End-to-end flow:
1. Call POST /advisory/interpretations/draft (passes observation_ids + question)
2. Receive draft AdvisoryResponse (advisory, not persisted)
3. Operator reviews and calls POST /advisory/interpretations to persist
   (sets capture_origin=operator_manual or claude_generated)
This flow is tested in test_advisory_interpretation.py.
Added additional test to confirm end-to-end works with the real provider interface.

### TF-B015: Contextual reasoning timelines
New `GET /advisory/reasoning-timeline` endpoint that composes:
- Replay timeline advisory entries (from event store)
- Interpretation analytics (from interpretation store)
Into a single chronological advisory reasoning timeline per decision.
Returns advisory-only content, distinguished from canonical lifecycle events.
