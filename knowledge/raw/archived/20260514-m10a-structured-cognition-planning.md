---
title: M10A Planning Capture — Structured Decision Authoring And Cognitive Capture
type: raw
status: raw
created: 2026-05-14
milestone: M10A
---

# M10A Planning Capture

## Trigger

User attempted to create a thesis from an existing trade idea in the operational system.
Discovered that "Develop Thesis" fires an immediate empty lifecycle transition (payload: {}).
No form, no structured content capture, no thesis narrative preserved in the event ledger.

## The Gap

TradeThesis is canonically defined as:
- a structured rationale with hypothesis, assumptions, invalidation conditions, confidence

In practice the system stores:
- a bare `decision.thesis_created` event with `payload: {}`
- no operator reasoning is preserved
- replay cannot reconstruct the thesis that existed before planning

## Architectural Significance

This gap matters because:
1. The thesis is the cognitive foundation for plan creation
2. Without it, plan review is comparing a plan to nothing
3. Replay shows a "Thesis" lifecycle marker with no content
4. Review workspaces cannot compare original thesis against outcome
5. Future AI advisory systems will need structured historical cognition to augment

## Selected Implementation Approach

Cognitive artifacts are stored as structured event payload data.
No separate storage tables for MVP (strict event sourcing).
Dedicated API endpoints validate required artifact fields before creating events.
Frontend modal forms replace immediate empty transitions.

## Key Domain Decisions

1. `ThesisArtifact` domain model: narrative, catalysts (list), assumptions (list),
   invalidation_conditions (list), confidence_level (1-5), regime_alignment (optional)

2. `POST /lifecycle/decisions/develop-thesis` endpoint:
   - validates required fields
   - creates decision.thesis_created event with structured payload
   - follows pattern of POST /lifecycle/decisions/init (TF-0053)

3. `ThesisDevelopmentModal` frontend component:
   - opens on "Develop Thesis" click (replaces immediate transition)
   - form fields: narrative (textarea), catalysts (dynamic list), assumptions (dynamic list),
     invalidation_conditions (dynamic list), confidence_level (1-5 select)
   - submits to dedicated endpoint
   - navigates to plan-review on success

4. Plan Review workspace: add thesis_content read surface showing narrative and catalysts
   for context when creating a plan

5. Replay timeline: existing payload field already carries content; frontend can display
   thesis fields inline for decision.thesis_created events

## Replay Implications

- No new replay infrastructure needed for M10AIS01-02
- Existing GET /replay/timeline already returns full event payloads
- Frontend Replay workspace updated in M10AIS09 to surface thesis content
- Graceful degradation for legacy empty-payload events

## Lifecycle Implications

- Idea→Thesis transition gated by ThesisDevelopmentModal
- decision.thesis_created event enriched with structured payload
- No lifecycle authority changes (transition validation unchanged)
- Plan→Approval and later stages unchanged for M10AIS01-02 scope

## Event Model Implications

- decision.thesis_created event payload gains structured thesis fields
- Backward compatibility: empty payload events remain valid in event store
- No event schema versioning required for MVP; optional field pattern

## Testing Strategy

Backend:
- POST /lifecycle/decisions/develop-thesis validates required fields
- POST /lifecycle/decisions/develop-thesis creates decision.thesis_created event
- POST /lifecycle/decisions/develop-thesis rejects missing narrative
- POST /lifecycle/decisions/develop-thesis rejects empty catalysts list

Frontend:
- TypeScript typecheck passes for ThesisDevelopmentModal
- ThesisDevelopmentModal renders correctly (manual test)
- Form validation prevents empty submission

## ADR Decisions Made

- ADR-0033: Structured Cognitive Artifact Model
- ADR-0034: Thesis And Plan Authoring Architecture
- ADR-0035: Replay Cognitive Reconstruction Strategy

## Issue Scope For This Cycle

- M10AIS01: Structured Thesis Domain Model (backend model + API endpoint)
- M10AIS02: Thesis Authoring Workspace (frontend modal + opportunity workspace update)

Deferred to subsequent M10A issues:
- M10AIS03: Thesis Revision History
- M10AIS04-05: Scenario Branch Modeling
- M10AIS06-08: Structured Trade Planning
- M10AIS09-15: Replay enrichment, review enrichment, annotations, playbook alignment

## Open Questions

- Should thesis revisions use a separate event type (decision.thesis_revised)?
  Yes — immutability means edits become new events, not mutations. Deferred to M10AIS03.
- Should catalysts/assumptions be free-text list items or tagged/structured?
  Free-text for MVP; structured tagging in future M10A iterations.
- Should confidence_level be numeric (1-5) or semantic (low/medium/high)?
  Numeric (1-5) for MVP — maps naturally to future RL/adaptive systems.
