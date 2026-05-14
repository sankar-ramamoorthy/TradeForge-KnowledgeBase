---
title: M10A Structured Decision Authoring — Planning Synthesis
type: processed
status: processed
created: 2026-05-14
milestone: M10A
source: knowledge/raw/20260514-m10a-structured-cognition-planning.md
---

# M10A Structured Decision Authoring — Planning Synthesis

## Summary

M10A was triggered by a direct system test: the user attempted to create a thesis from an
existing trade idea and discovered the "Develop Thesis" action fires an empty, contentless
lifecycle transition.

The gap: lifecycle stages are workflow markers, not cognitive artifacts. TradeThesis is
semantically defined but structurally empty in the event store.

## Durable Architectural Decision

Cognitive artifacts are persisted as structured event payload data inside immutable lifecycle
events. This is the canonical pattern for M10A:

```
operator authoring form
    → validated API endpoint
    → lifecycle transition with structured payload
    → immutable event in ledger
    → projection extracts artifact for display
```

No separate artifact storage tables. Event sourcing purity preserved.

## Promoted Entities

- [[StructuredThesis]] — canonical thesis artifact model (KB-M10A-001)
- [[StructuredTradePlan]] — canonical plan artifact model (KB-M10A-002)
- [[ScenarioBranch]] — conditional reasoning structure (KB-M10A-003)
- [[ReviewReflection]] — post-decision learning artifact (KB-M10A-004)
- [[CognitiveSnapshot]] — point-in-time cognitive reconstruction (KB-M10A-005)
- [[DecisionArtifact]] — parent concept for all cognitive artifacts

## ADR Decisions

- ADR-0033: Structured Cognitive Artifact Model — artifact persistence strategy
- ADR-0034: Thesis And Plan Authoring Architecture — authoring workflow design
- ADR-0035: Replay Cognitive Reconstruction Strategy — replay surface for artifacts

## Implementation Scope (M10AIS01-02)

M10AIS01 scope (backend):
- `src/domain/cognition/thesis.py` — ThesisArtifact domain dataclass
- `POST /lifecycle/decisions/develop-thesis` endpoint with validation
- Backend tests for endpoint behavior

M10AIS02 scope (frontend):
- `ThesisDevelopmentModal.tsx` — form capturing thesis fields
- `OpportunityWorkspace.tsx` — uses modal instead of empty transition
- `frontend/src/api/runtime.ts` — `postDevelopThesis` API function

## Deferred Work (Subsequent M10A Issues)

- M10AIS03: Thesis revision history (decision.thesis_revised event)
- M10AIS04-05: ScenarioBranch modeling and visualization
- M10AIS06-08: StructuredTradePlan domain model + plan authoring workspace
- M10AIS09-10: Replay cognitive artifact timeline + snapshot reconstruction
- M10AIS11-12: ReviewReflection model + review workspace enrichment
- M10AIS13: Replay annotation system
- M10AIS14: Playbook alignment projection
- M10AIS15: Cross-workspace cognitive continuity
