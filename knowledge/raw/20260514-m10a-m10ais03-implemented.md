---
title: M10AIS03 Implementation Capture — Thesis Revision History
type: raw
status: raw
created: 2026-05-14
milestone: M10A
issue: M10AIS03
---

# M10AIS03 — Thesis Revision History Implementation Capture

## What Was Implemented

### Backend

New event type: `decision.thesis_revised`
- Same payload schema as thesis_created
- NOT a lifecycle stage transition — stage stays at Thesis
- event_store.append() called directly, not via LifecycleOrchestrationService

New endpoint: `POST /lifecycle/decisions/revise-thesis`
- Same validation as develop-thesis
- Checks current lifecycle stage is Thesis before allowing revision
- Counts existing thesis events to set revision_number
- Returns: decision_id, event_type, timestamp, revision_number

Updated: `GET /lifecycle/decisions/{decision_id}/thesis`
- Now scans both thesis_created and thesis_revised events
- Returns the most recent one (reversed scan)

New endpoint: `GET /lifecycle/decisions/{decision_id}/thesis/history`
- Returns all thesis snapshots in chronological order
- Each snapshot includes revision_number, event_type, event_timestamp, all thesis fields
- ThesisHistoryResponse: {decision_id, total_revisions, snapshots: [ThesisSnapshotResponse]}

Top-level imports added: LIFECYCLE_EVENT_STAGE_MAP, EventEnvelope (cleanup)

7 new backend tests (541 total):
- revise creates thesis_revised event
- revise returns correct revision_number
- GET /thesis returns latest after revision
- GET /thesis/history returns all snapshots in order
- revise rejects when not in Thesis stage
- revise rejects empty narrative
- history is in chronological order

### Frontend

New file: `ThesisRevisionModal.tsx`
- Pre-populated with current thesis values
- Same form structure as ThesisDevelopmentModal
- POSTs to /lifecycle/decisions/revise-thesis
- On success: closes modal and re-fetches thesis to update ThesisContextPanel

Updated: `ThesisContextPanel` (in PlanReviewWorkspace.tsx)
- Accepts `canRevise` and `onRevise` props
- Shows "Revise Thesis" button when canRevise (stage = Thesis)
- Shows "— Revised" badge when source_event_type is thesis_revised

Updated: `PlanReviewWorkspace.tsx`
- showRevisionModal state
- ThesisRevisionModal rendered when showRevisionModal + thesis available
- ThesisContextPanel receives canRevise/onRevise props

## Architecture Observations

- Decision: appendRevisionEvent directly to event store (not via lifecycle service) because thesis revision is NOT a lifecycle stage transition. The lifecycle service would reject it as an invalid transition.
- Consequence: revision events are in the event store but not in LIFECYCLE_EVENT_STAGE_MAP — they do not affect lifecycle state derivation. This is correct behavior.
- The `from_payload()` method works identically for both thesis_created and thesis_revised — the payload schema is identical.

## Test Verification

- uv run pytest: 541 passed
- npm run typecheck: clean
- npm run build: 282.66 kB JS, clean
