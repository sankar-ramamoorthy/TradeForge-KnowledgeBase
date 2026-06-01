---
title: M14C TF-R001 Implementation Plan
type: design-plan
status: draft
created: 2026-05-27
updated: 2026-05-27
tags:
  - TradeForge
  - M14C
  - TF-R001
  - implementation-planning
  - Thesis-Workspace
  - advisory-artifact
  - replayability
sources:
  - "knowledge/processed/20260527-m14c-operator-cognition-bridge.md"
  - "knowledge/design/M14C_THESIS_IMPORT_WORKFLOW.md"
related_milestones:
  - M12
  - M14C
related_issues:
  - TF-A016
  - TF-A017
  - TF-A018
  - TF-A019
  - TF-A020
  - TF-R001
related:
  - [[M14C Operator Cognition Bridge]]
  - [[M14C Thesis Import Workflow]]
  - [[Advisory Artifact]]
  - [[Decision Lifecycle Engine]]
  - [[Event Ledger]]
  - [[Replay System]]
---

# M14C TF-R001 Implementation Plan

## Summary

`TF-R001` is the first thin M14C implementation slice.

The goal is to prove that a Markdown/YAML thesis draft can move through the existing M12 advisory artifact substrate into the Thesis Workspace as advisory cognition, support selective field acceptance and operator edits, and preserve replay-visible provenance after explicit manual Thesis submission.

This is implementation planning, not code design. Framework-level details remain out of scope.

## Scope

Thin path:

```text
Markdown/YAML thesis draft
    -> M12 advisory artifact substrate
    -> Thesis Workspace import preview
    -> selective field acceptance
    -> operator edits
    -> manual Thesis submission
    -> replay-visible provenance
```

Included:

- `thesis_draft` schema mapping
- advisory artifact mapping
- one Thesis Workspace import preview path
- field-level acceptance for a small set of thesis fields
- provenance markers on accepted fields
- edited-after-import indication
- explicit manual Thesis submission
- post-submit traceability to source advisory artifact

## Non-Goals

`TF-R001` must not design or implement:

- universal import framework
- background watchers
- autonomous lifecycle creation
- plan import
- execution import
- AI parsing pipeline
- broker integration
- automatic conviction assignment
- automatic sizing decisions
- automatic approval
- automatic execution authorization
- hidden lifecycle transitions

## Dependency Alignment With M12

`TF-R001` should reuse the completed M12 advisory substrate:

- `TF-A016` external Research Cockpit import boundary
- `TF-A017` research artifact ingestion API
- `TF-A018` generated advisory artifact support
- `TF-A019` advisory markdown artifact persistence
- `TF-A020` replay-safe advisory snapshot capture

M14C should not re-create advisory artifact persistence or source ingestion as a parallel system.

The implementation planning question is:

```text
How should a persisted advisory artifact be presented and selectively incorporated
inside the Thesis lifecycle workflow?
```

## Proposed Thesis Draft Schema

Initial schema should be narrow and versioned.

Conceptual shape:

```yaml
artifact_type: thesis_draft
schema_version: 1
symbol: ATKR
title: Aerospace recovery thesis
source:
  system: claude
  model: optional-model-name
  generated_at: 2026-05-27T00:00:00Z
thesis:
  narrative: >
    Draft thesis narrative...
catalysts:
  - catalyst text
risks:
  - risk text
assumptions:
  - assumption text
invalidation:
  - invalidation condition
evidence:
  - label: source label
    reference: source reference
notes:
  - operator or source note
```

Schema constraints:

- must identify artifact type
- must identify schema version
- should include symbol where known
- should preserve source/provenance where known
- must not include execution authorization
- must not include sizing authority
- must not encode approval

## Advisory Artifact Mapping

The thesis draft should map into an `Advisory Artifact`, not a `Thesis`.

Conceptual mapping:

```text
Markdown/YAML source
    -> advisory artifact body / metadata
    -> artifact type: thesis_draft
    -> origin: imported_research, claude_generated, codex_generated, or operator-supplied
    -> provenance: source system, timestamp, source reference
    -> mapped thesis fields for preview
```

Mapped fields remain advisory until accepted into draft. Accepted draft fields remain non-canonical until manual Thesis submission.

## Thesis Workspace Integration Points

The Thesis Workspace should integrate at three conceptual points:

1. Import availability
   - show when eligible advisory artifacts exist for the current symbol or candidate

2. Import preview
   - inspect source artifact and mapped fields

3. Draft incorporation
   - accept fields into editable Thesis draft authoring surface

Submission remains the existing Thesis workflow action.

The import preview should not own lifecycle submission.

## Field Acceptance Mechanics

Initial field set:

- title
- thesis narrative
- catalysts
- risks
- assumptions
- invalidation
- evidence links
- notes

Acceptance mechanics:

```text
Advisory field
    -> operator selects Accept
    -> field content appears in draft
    -> field receives provenance marker
    -> operator may edit
    -> field remains non-canonical until Thesis submission
```

Existing draft conflict behavior for first slice can be simple:

- require explicit append or replace
- defer compare/merge if needed

Do not auto-merge conflicting thesis meaning.

## Provenance Persistence Considerations

Minimum provenance needed:

- advisory artifact ID or source reference
- origin type
- source system/tool where available
- captured/imported timestamp
- accepted fields
- edited-after-import indicator where feasible
- final canonical event reference after submission

Open persistence decision:

- whether field acceptance is persisted before submission
- whether field acceptance is captured only in draft metadata until submitted
- whether rejected/deferred field states are durable or session-local

`TF-R001` should choose the simplest option that preserves replay clarity for the promoted thesis.

## Replay Traceability Considerations

Replay should answer:

- Which advisory artifact influenced this thesis?
- Which fields were accepted from import?
- Which accepted fields were edited?
- What canonical lifecycle event resulted?

Thin-slice replay may use:

- advisory artifact snapshot from M12
- thesis lifecycle event provenance references
- field provenance metadata attached to submitted thesis payload or associated projection

Replay must not treat the advisory artifact itself as canonical thesis truth.

## Suggested Event / Projection Considerations

Canonical events:

- normal lifecycle Thesis event only after operator submission

Non-canonical/projection concepts:

- imported artifact field mapping
- draft field provenance
- accepted field list
- edited-after-import status
- replay provenance overlay

Open event question:

- Should `TF-R001` add new event types for field acceptance, or avoid new events and rely on advisory artifact persistence plus final lifecycle-event traceability?

Planning preference:

- avoid new canonical event types unless replay cannot be made clear without them
- do not encode advisory content as canonical event truth

## UX Constraints

The UI must:

- distinguish advisory source material from draft thesis content
- distinguish draft content from canonical lifecycle state
- keep imported material editable
- show provenance before submission
- preserve normal Thesis submission semantics
- avoid approval, sizing, or execution language

The UI must not:

- auto-create a thesis
- hide imported origins
- treat acceptance as canonical promotion
- merge imported text into existing draft text without operator choice

## Canonical Boundary Protections

Required protections:

- import preview cannot submit lifecycle events
- field acceptance cannot create lifecycle events
- operator submission must use normal lifecycle validation
- source advisory artifact remains non-canonical
- final canonical event must be operator-owned
- no automatic conviction, sizing, approval, or execution fields

Boundary phrase:

```text
Accepted into draft does not mean promoted to lifecycle.
Promoted to lifecycle requires explicit operator submission.
```

## Thin Vertical Slice Definition

Minimum successful `TF-R001` demonstration:

```text
1. A thesis_draft advisory artifact exists.
2. Operator enters Develop Thesis from Opportunity Workspace.
3. Thesis Workspace surfaces one eligible import preview.
4. Operator accepts narrative, catalysts, and invalidation.
5. Operator edits narrative.
6. Operator rejects or ignores risks.
7. Operator submits Thesis manually.
8. Resulting canonical lifecycle event references source provenance.
9. Replay or replay-adjacent view distinguishes source advisory cognition from canonical Thesis event.
```

## Suggested Issue Breakdown

If `TF-R001` is split internally:

- `TF-R001a - Define thesis draft schema mapping`
- `TF-R001b - Surface eligible advisory artifacts in Thesis Workspace`
- `TF-R001c - Implement field acceptance draft mechanics`
- `TF-R001d - Add provenance indicators and edited-after-import state`
- `TF-R001e - Preserve provenance through manual Thesis submission`
- `TF-R001f - Validate replay traceability for promoted thesis`

The runtime issue register should decide whether these are sub-tasks or separate issues.

## Operational Risks

- UI may make import feel like automatic thesis generation.
- Accepted fields may be mistaken for canonical state.
- Provenance may become too hidden to support trust.
- Too much provenance may overload authoring cognition.
- Missing field-level replay may weaken later cognitive reconstruction.
- Over-instrumenting field states may create unnecessary complexity.
- Plan-like fields may slip into thesis import and imply execution authority.

## Open Technical Questions

- Does existing M12 advisory artifact persistence support the needed thesis field mapping directly?
- Where should accepted-field provenance live before submission?
- Should edited-after-import be computed by content comparison or explicit edit tracking?
- What is the minimum submitted-thesis provenance payload?
- Should rejected/deferred import decisions persist?
- Should previewed state be durable or session-local?
- How should multiple artifacts for one symbol be ordered?
- Does the runtime already have a suitable draft thesis representation, or is a thin projection needed?

