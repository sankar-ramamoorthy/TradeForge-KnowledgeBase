# TF-B002 through TF-B009 — Interpretation Services Planning Note

## Date
2026-05-22

## Issues
- TF-B002: Contextual weighting service
- TF-B003: Regime-aware weighting model
- TF-B004: Conflicting evidence analysis
- TF-B005: Confidence-range API surface
- TF-B006: Thesis evidence influence tracking extension
- TF-B007: Supporting vs weakening evidence classification surface
- TF-B008: Thesis drift detection
- TF-B009: Contextual contradiction surfacing

## Foundation From M12
All domain types are in place:
- ContextualWeight enum (low/medium/high/watch)
- AdvisoryConfidenceRange enum (low/medium/high/unknown)
- ThesisInfluence enum (supporting/weakening/conflicting/mixed/neutral/unknown)
- EvidenceConflictMarker (conflicting/mixed/unresolved)
- AdvisoryInterpretation with all fields
- AdvisoryInterpretationQueryService.thesis_influence_summary()
- Postgres and InMemory stores

## Approach

### TF-B002 + TF-B005: Extend AdvisoryInterpretationQueryService
The existing `AdvisoryInterpretationQueryService` already has
`thesis_influence_summary()`. Extend it with:
- `contextual_weight_distribution()` — counts interpretations by
  ContextualWeight for a given query
- `confidence_range_distribution()` — counts interpretations by
  AdvisoryConfidenceRange

These are advisory read-model summaries, non-canonical.

### TF-B006 + TF-B007: Interpretation influence timeline
Add `interpretation_influence_timeline()` — returns interpretations
for a given thesis in chronological order with their influence label.
This gives "influence over time" rather than just current counts.

Also add filtering by `ThesisInfluence.SUPPORTING` or `WEAKENING` via
the existing query service `list()` method (already supports thesis_influence
filter). Add API endpoints that expose supporting/weakening split clearly.

### TF-B003: Regime-aware weighting context
Add `RegimeContextWeight` service that suggests `ContextualWeight` adjustment
based on current market regime. Uses `MarketRegime` enum from market domain.
Advisory only — the suggestion doesn't auto-apply; the operator decides.

Output: `RegimeWeightSuggestion` dataclass with suggested weight and rationale.
This feeds into the interpretation capture workflow to help operators assign
contextual weight with regime context in mind.

### TF-B004 + TF-B009: Conflict detection service
Add `ConflictAnalysisService` that detects when observations have conflicting
evidence markers (`EvidenceConflictMarker`) or when interpretations have
opposing `ThesisInfluence` values (e.g., SUPPORTING + WEAKENING present
for the same thesis). Returns `ConflictSummary` with conflict count,
unresolved count, and the specific conflicting interpretation IDs.

### TF-B008: Drift detection service
Add `ThesisDriftDetectionService` that analyzes interpretation history
and detects when the balance of influence has shifted. Specifically:
- Was previously majority SUPPORTING (>60% of interpretations)
- Is now majority WEAKENING (>50%) or CONFLICTING
- Returns `ThesisDriftSignal` with drift_detected bool, previous_dominant,
  current_dominant, interpretation_count, and supporting_ids

All of the above are services on top of the existing domain + stores.
No new domain model changes needed. No new ADRs needed.

## API Additions (in routes.py)
- GET /advisory/interpretations/weight-distribution
- GET /advisory/interpretations/confidence-distribution
- GET /advisory/interpretations/influence-timeline
- GET /advisory/conflict-summary
- GET /advisory/drift-signal

## Testing
Primarily service-level unit tests with InMemory stores.
