---
title: M14C Operational Testing And Replay Validation
type: validation-plan
status: draft
created: 2026-05-27
updated: 2026-05-27
tags:
  - TradeForge
  - M14C
  - operational-testing
  - replay-validation
  - lifecycle-integrity
  - provenance
  - Human-Decision-Sovereignty
sources:
  - "knowledge/processed/20260527-m14c-operator-cognition-bridge.md"
  - "knowledge/design/M14C_THESIS_IMPORT_WORKFLOW.md"
related_milestones:
  - M12
  - M14C
  - M15
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

# M14C Operational Testing And Replay Validation

## Summary

This document defines how the M14C Thesis import workflow should be operationally validated before expansion beyond `TF-R001`.

The goal is to prove cognitive clarity, provenance correctness, replay correctness, lifecycle integrity, canonical boundary protection, and operator trust.

The central validation question:

```text
Can replay clearly reconstruct source cognition, accepted fields, operator edits,
promoted lifecycle state, and canonical lifecycle events without confusing
advisory cognition with lifecycle truth?
```

## Validation Goals

- Verify imported research remains advisory until explicit operator promotion.
- Verify field acceptance does not mutate lifecycle state.
- Verify manual Thesis submission remains the canonical promotion point.
- Verify provenance is visible during authoring and replay.
- Verify operator edits are distinguishable from source content.
- Verify rejected and deferred imports do not become canonical.
- Verify replay distinguishes advisory source material from canonical lifecycle events.
- Verify workflow remains cognitively useful rather than automation-like.

## Canonical Boundary Validation

Boundary tests should confirm:

- no thesis is created when an advisory artifact is ingested
- no thesis is created when an import preview is opened
- no thesis is created when fields are accepted into draft
- no lifecycle event is appended until explicit Thesis submission
- no approval, execution authorization, conviction, or sizing is inferred
- final Thesis event is operator-owned and lifecycle-authorized

Manual validation prompts:

- Does any UI label imply the import is already a thesis?
- Does any action from the import preview create lifecycle state?
- Can the operator distinguish `Accepted` from `Promoted`?
- Can the operator continue authoring without using imported material?

## Replay Reconstruction Validation

Replay must reconstruct:

- source advisory artifact
- source provenance
- accepted fields where captured
- edited-after-import fields where captured
- operator-submitted thesis
- canonical lifecycle event

Replay must not:

- present imported research as canonical before submission
- treat rejected material as lifecycle intent
- require live external APIs or source files
- depend on mutable UI state

Replay validation scenario:

```text
Import advisory thesis draft
    -> accept narrative and catalysts
    -> edit narrative
    -> reject risks
    -> submit Thesis
    -> replay decision history
```

Expected replay result:

- advisory artifact appears as source cognition
- narrative appears as imported then edited
- catalysts appear as imported/accepted
- risks appear rejected or unused if captured
- Thesis event appears as canonical lifecycle fact

## Provenance Validation

Provenance should be validated at two levels.

Artifact-level:

- artifact ID or source reference is visible
- origin type is visible
- captured/imported timestamp is visible
- source system/tool is visible where known
- caveats or uncertainty are visible where available

Field-level:

- accepted fields show source markers
- edited accepted fields show edited-after-import markers
- operator-authored fields are distinguishable
- mixed fields can be identified or explained

Validation question:

```text
Can an operator understand where each important thesis field came from
without losing the thread of thesis authoring?
```

## Operator Cognition Validation

Testing should evaluate whether the workflow supports deliberate reasoning.

Observe:

- Does the operator inspect source provenance before accepting content?
- Does field-level acceptance encourage selective incorporation?
- Does the operator edit imported content into their own words or intent?
- Does the UI make final submission feel deliberate?
- Does the operator understand that they remain responsible for the thesis?

Failure signs:

- operator treats imported content as system recommendation
- operator submits without understanding provenance
- UI feels like AI auto-completion
- source caveats are ignored because they are too hidden
- provenance overwhelms authoring and causes avoidance

## UX Friction Evaluation

Friction should be evaluated carefully. Some friction is intentional because lifecycle promotion must remain deliberate.

Useful friction:

- explicit field acceptance
- explicit append/replace choice
- visible provenance before submission
- explicit Thesis submission

Harmful friction:

- import preview blocks normal thesis writing
- provenance requires too many clicks to inspect
- accepting several fields requires repetitive low-value actions
- rejected material cannot be recovered
- operator cannot tell what changed after editing

## Failure And Recovery Testing

Test failure states:

- no eligible import material
- malformed thesis draft schema
- unsupported artifact type
- missing symbol
- ambiguous symbol
- stale artifact
- duplicate artifact
- artifact source unavailable after import
- draft conflict with existing text

Expected behavior:

- Thesis authoring remains available
- lifecycle state remains unchanged
- error is presented as import/advisory problem
- operator can dismiss, defer, or continue manually
- no invalid canonical event is written

## Rejection / Deferred Import Testing

Rejection cases:

- reject entire artifact for this workflow
- reject a field
- remove an accepted field from draft before submission

Deferred cases:

- defer artifact while continuing manual authoring
- return to deferred artifact later

Validation questions:

- Does rejection clearly mean "not used in this workflow" rather than deletion?
- Does deferred material remain non-canonical?
- Is rejection replay-visible, session-local, or projection-only?

The final persistence semantics remain an open design question.

## Edited-After-Import Validation

Test flow:

```text
Accept imported narrative
    -> edit wording and meaning
    -> submit Thesis
    -> inspect provenance
    -> replay decision
```

Expected result:

- field remains linked to source artifact
- field is marked edited after import
- final text is treated as operator-authored lifecycle content
- source artifact is not presented as exact author of final field

Important distinction:

```text
Source influenced final thesis
does not mean
source authored final canonical thesis
```

## Multi-Artifact Collision Scenarios

Even if `TF-R001` starts with one artifact, test design should anticipate collisions.

Scenarios:

- two artifacts for same symbol
- one artifact strengthens thesis, another weakens it
- duplicate artifacts from different tools
- artifact conflicts with existing draft text
- artifact contains stale or contradictory evidence

Validation goals:

- operator can identify source differences
- system does not auto-rank one artifact as authoritative
- field acceptance remains selective
- conflicting cognition remains visible rather than hidden

Multi-artifact handling may be deferred, but collision behavior should not contradict M14C doctrine.

## Replay Narrative Validation

Replay should tell a coherent decision story:

```text
The operator considered imported advisory cognition.
The operator accepted selected fields into a thesis draft.
The operator edited part of the imported reasoning.
The operator submitted the thesis through normal lifecycle workflow.
The Event Ledger contains the canonical lifecycle fact.
The advisory artifact remains source cognition, not canonical truth.
```

Replay should support review questions:

- What did the operator know?
- What source material influenced the thesis?
- What did the operator change?
- What did the operator reject or ignore, if captured?
- Was the canonical thesis operator-owned?

## Thin Slice Operational Test Cases

Test Case 1: Happy path

- one valid thesis draft artifact
- accept narrative, catalysts, invalidation
- edit narrative
- submit Thesis
- verify provenance and replay

Test Case 2: Manual authoring without import

- eligible import exists
- operator ignores import
- manually writes thesis
- submit Thesis
- verify no false import provenance

Test Case 3: Reject import

- eligible import exists
- operator rejects artifact
- manually writes thesis
- verify rejected artifact does not become canonical

Test Case 4: Existing draft conflict

- draft field has text
- operator accepts imported narrative
- choose append or replace
- submit
- verify chosen behavior and provenance

Test Case 5: Edited-after-import

- accept imported field
- substantially edit it
- submit
- verify edited provenance and replay story

Test Case 6: Invalid artifact

- malformed or unsupported artifact
- show import error
- continue manual Thesis workflow
- verify no lifecycle mutation from error

## Suggested Manual Testing Workflow

Manual test script:

```text
1. Seed or create one thesis_draft advisory artifact.
2. Open Opportunity Workspace for the same symbol.
3. Select Develop Thesis.
4. Confirm Thesis Workspace shows import preview as advisory.
5. Inspect provenance.
6. Accept narrative, catalysts, and invalidation.
7. Edit narrative.
8. Reject risks.
9. Submit Thesis manually.
10. Inspect resulting lifecycle state.
11. Open replay/review view.
12. Confirm source cognition and canonical event are distinguishable.
```

Tester notes should capture:

- where the workflow felt clear
- where advisory/canonical distinction was ambiguous
- whether provenance was visible enough
- whether submission felt deliberate
- whether replay told the correct story

## Expansion Readiness Criteria

M14C should not expand to Plan or Review imports until:

- `TF-R001` proves lifecycle boundaries are clear
- operators can distinguish import acceptance from canonical promotion
- replay reconstructs source cognition and canonical thesis state clearly
- provenance is visible without overwhelming authoring
- rejected/deferred import behavior is understood
- edited-after-import semantics are clear
- no UI path suggests automatic approval, sizing, or execution

ADR readiness may be considered when:

- UX pattern stabilizes
- persistence semantics are clear
- replay requirements are met
- cross-workspace reuse implications are understood

## Open Validation Questions

- What exact replay view is required for `TF-R001` acceptance?
- Should field acceptance be validated through event history, projection state, or submitted thesis provenance?
- Should rejected/deferred imports be durable?
- How much provenance must be visible before submission?
- What threshold of operator confusion should block expansion?
- How should validation detect whether the workflow feels like automation?
- Should multi-artifact collision be included in `TF-R001` validation or deferred?

