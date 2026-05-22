---
title: M10AIS03 Planning — Thesis Revision History
type: raw
status: raw
created: 2026-05-14
milestone: M10A
issue: M10AIS03
---

# M10AIS03 Planning — Thesis Revision History

## Scope

Preserve thesis evolution over time as immutable revision snapshots.
Replay can reconstruct what the thesis was at any point before plan creation.

## Key Design Decision

Thesis revision is NOT a lifecycle transition. The lifecycle stage remains Thesis.
Revision is an enrichment event — a cognitive update that records how operator
reasoning evolved without advancing the workflow.

## Event Model

New event type: `decision.thesis_revised`
- Same payload schema as thesis_created: {thesis: {narrative, catalysts, assumptions, invalidation_conditions, confidence_level, regime_alignment}}
- Does NOT appear in LIFECYCLE_EVENT_STAGE_MAP (not a stage transition)
- Does NOT appear in LIFECYCLE_STAGE_EVENT_TYPE_MAP (not a canonical stage event)
- Just appended to the event store as any other event

## ADR Checkpoint

No new ADR needed. ADR-0033 mentions revision as natural extension.
ADR-0035 covers replay reconstruction from event payloads.

## Backend Changes

1. `POST /lifecycle/decisions/revise-thesis` — body pattern consistent with develop-thesis
   - requires `decision_id` and full thesis fields
   - validates thesis content (same rules)
   - checks that the current lifecycle stage is Thesis (revision only valid at Thesis stage)
   - creates `decision.thesis_revised` event with structured payload
   - does NOT call LifecycleOrchestrationService (not a lifecycle transition)
   - calls event_store.append() directly

2. `GET /lifecycle/decisions/{decision_id}/thesis` — update to scan both event types
   - currently only looks for `decision.thesis_created`
   - update to look for either `decision.thesis_created` OR `decision.thesis_revised`
   - return the most recent one (already reversed scan)

3. `GET /lifecycle/decisions/{decision_id}/thesis/history` — new endpoint
   - returns list of all thesis snapshots in chronological order
   - each item includes: narrative, catalysts, assumptions, invalidation_conditions, confidence_level, regime_alignment, event_type, event_timestamp, revision_number

## Frontend Changes

1. `ThesisRevisionModal.tsx` — reuse ThesisDevelopmentModal pattern
   - prepopulate with current thesis values
   - submit to POST /lifecycle/decisions/revise-thesis
   - show in PlanReviewWorkspace when stage is Thesis

2. `PlanReviewWorkspace.tsx` — add "Revise Thesis" button
   - appears when `canCreatePlan` (stage is Thesis) AND thesis exists
   - opens ThesisRevisionModal

3. `ThesisContextPanel` — add revision count indicator
   - show "Revision 1 of N" or "Initial thesis" or "Revised N times"
   - optionally show revision timestamp

## Replay Implications

No new replay infrastructure needed.
All thesis events (thesis_created + thesis_revised) are already in the event timeline.
GET /replay/timeline already returns their payloads.
The thesis history endpoint reconstructs the evolution from event history.

## Testing Strategy

Backend:
- POST /lifecycle/decisions/revise-thesis creates decision.thesis_revised event
- POST /lifecycle/decisions/revise-thesis rejects revision when not in Thesis stage
- GET /lifecycle/decisions/{id}/thesis returns latest after revision
- GET /lifecycle/decisions/{id}/thesis/history returns all snapshots in order
- Multiple revisions accumulate correctly

## Implementation Boundary

- Thesis revision only valid at Thesis stage (before Plan is created)
- Once lifecycle advances to Plan, no more revisions (immutable thesis at plan creation time)
- This is enforced by checking current lifecycle stage before appending revision event
