---
title: M14C Thesis Import Workflow
type: design
status: draft
created: 2026-05-27
updated: 2026-05-27
tags:
  - TradeForge
  - M14C
  - Thesis-Workspace
  - Import-Bridge
  - UX-workflow
  - advisory-artifact
  - Human-Decision-Sovereignty
  - replayability
sources:
  - "knowledge/processed/20260527-m14c-operator-cognition-bridge.md"
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
  - [[Advisory Artifact]]
  - [[Decision Lifecycle Engine]]
  - [[Event Ledger]]
  - [[Replay System]]
  - [[Persona Workspace]]
---

# M14C Thesis Import Workflow

## Summary

This document operationalizes the M14C Operator Cognition Bridge for the Thesis Workspace.

The operator is not "importing a thesis." The operator is reviewing imported advisory cognition, selectively incorporating reasoning, authoring lifecycle intent, and explicitly promoting canonical lifecycle state through the existing Thesis workflow.

Imported research artifacts remain advisory cognition. Accepted draft material remains non-canonical until the operator submits the Thesis workflow through the normal lifecycle authority path.

## Relationship To M14C Doctrine

This design follows [[M14C Operator Cognition Bridge]] as doctrinal authority.

M14C is not greenfield ingestion infrastructure. It extends completed M12 advisory artifact foundations:

- `TF-A016 - Define external Research Cockpit import boundary`
- `TF-A017 - Implement research artifact ingestion API`
- `TF-A018 - Implement Codex/Claude-generated advisory artifact support`
- `TF-A019 - Implement advisory markdown artifact persistence`
- `TF-A020 - Implement replay-safe advisory snapshot capture`

The Thesis import workflow sits above that substrate. It defines how imported advisory artifacts become operator-reviewed draft inputs without becoming canonical lifecycle truth.

## Workflow Goals

- Preserve the existing Opportunity -> Develop Thesis lifecycle entry.
- Keep the operator in control of lifecycle intent.
- Make imported advisory cognition inspectable before use.
- Support selective field acceptance rather than all-or-nothing import.
- Preserve provenance at artifact and field level.
- Make advisory, draft, edited, promoted, and canonical states visually distinct.
- Preserve replayability of source cognition and operator incorporation.
- Avoid hidden lifecycle transitions.

The workflow should feel like structured reasoning assistance, lifecycle-aware cognition handling, and advisory evidence review.

It should not feel like autocomplete trading, AI-driven lifecycle automation, or hidden canonical mutation.

## Operator Mental Model

The operator should understand the workflow as:

```text
I found or generated useful research.
TradeForge shows it as advisory source material.
I inspect what it says and where it came from.
I choose which reasoning belongs in my thesis draft.
I edit it into my own lifecycle intent.
I submit the Thesis workflow explicitly.
Only then does canonical lifecycle state change.
```

The system should reinforce four distinct layers:

- advisory source material
- accepted draft material
- operator-authored modifications
- canonical lifecycle state

## Opportunity Workspace Entry Flow

The import workflow starts from the existing Opportunity -> Develop Thesis path.

Entry conditions:

- The operator is in an Opportunity Workspace or equivalent opportunity/candidate context.
- A relevant imported advisory artifact exists in the M12 advisory substrate.
- The operator chooses to develop a thesis for a symbol, candidate, or opportunity.

Entry flow:

```text
Opportunity Workspace
    -> operator selects Develop Thesis
    -> Thesis Workspace opens in lifecycle authoring mode
    -> eligible import material is surfaced as advisory cognition
    -> operator reviews, accepts, edits, or ignores material
    -> operator submits Thesis workflow explicitly
```

The presence of an eligible import must not create a Thesis automatically. The import appears as available cognition, not as lifecycle state.

## Develop Thesis Interaction Flow

The Thesis Workspace should preserve deliberate cognition.

Primary interaction sequence:

```text
1. Enter Develop Thesis workflow
2. See existing lifecycle context and empty or existing thesis draft fields
3. See global import preview if eligible advisory material exists
4. Inspect imported artifact provenance and content
5. Accept selected fields or sections into the thesis draft
6. Edit accepted material into operator-authored reasoning
7. Review provenance and edited-field indicators
8. Submit Thesis explicitly
9. System appends normal canonical lifecycle event
10. Replay can show source advisory cognition and operator promotion path
```

The Thesis Workspace should not interrupt authoring if the operator ignores imports. Imports are optional aids.

## Global Import Preview UX

The global import preview is the boundary-preserving surface for advisory cognition.

It should show:

- artifact title or source label
- symbol or instrument reference when available
- source system or origin
- captured/imported timestamp
- artifact type, such as thesis draft or research note
- uncertainty, caveats, or provenance summary where available
- available sections mapped to thesis fields
- clear advisory/non-canonical status

The preview should support:

- inspect artifact
- accept field
- accept section
- reject artifact for this workflow
- defer artifact
- view source/provenance details

The preview should not include lifecycle submission controls. Lifecycle promotion belongs to the Thesis workflow, not the import preview.

## Selective Field Acceptance UX

Selective field acceptance is the primary interaction pattern.

The operator may accept:

- a single field
- a grouped field set
- a section
- all mapped fields, if explicitly chosen

Candidate field mappings:

- symbol
- thesis title
- thesis narrative
- catalysts
- risks
- assumptions
- invalidation conditions
- evidence links
- notes

Acceptance copies or references advisory content into draft authoring space. It does not create canonical lifecycle state.

The UX should make acceptance deliberate:

```text
Imported field
    -> operator accepts
    -> draft field receives imported content
    -> provenance marker appears
    -> field remains editable
    -> canonical state remains unchanged
```

If an operator accepts material into a field that already contains text, the UX should require a visible choice:

- append imported content
- replace draft content
- compare and merge
- cancel

The exact merge UI remains open.

## Field State Model

The field state model operationalizes the M14C vocabulary.

```text
Imported
    Advisory content exists in the import substrate.

Previewed
    Operator has viewed the imported artifact or field in the workflow.

Accepted
    Operator selected imported content for draft use.

Edited
    Operator modified accepted imported content.

Rejected
    Operator declined the imported artifact or field for this workflow.

Promoted
    Operator submitted the lifecycle workflow using draft material.

Canonical
    The resulting lifecycle event exists in the Event Ledger.
```

State flow:

```text
Imported
    -> Previewed
    -> Accepted
    -> Edited
    -> Promoted
    -> Canonical
```

Alternative paths:

```text
Imported -> Previewed -> Rejected
Imported -> Previewed -> Deferred
Accepted -> Rejected from draft
Accepted -> Edited -> Rejected from draft
```

Important distinction:

- `Accepted` means draft incorporation.
- `Promoted` means explicit lifecycle submission.
- `Canonical` means event-backed lifecycle truth.

Accepted is not canonical.

## Provenance Rendering Semantics

Provenance should be visible without overwhelming the authoring surface.

Artifact-level provenance should show:

- advisory artifact ID or stable source reference
- origin type, such as imported research, Codex-generated, Claude-generated, or operator-supplied
- source system or tool when known
- source file or external reference when available
- captured/imported timestamp
- schema or artifact type when available
- caveats and uncertainty where available

Field-level provenance should show:

- imported source indicator
- source artifact reference
- accepted timestamp or workflow-local acceptance marker where available
- edited-after-import indicator
- source evidence link if the field came from evidence material

Provenance should survive operator editing as attribution history, not as a claim that final text remains identical to the source.

Suggested field provenance distinctions:

```text
Imported unchanged
    Field content still matches accepted source material.

Imported then edited
    Field content originated from import but now contains operator modifications.

Operator-authored
    Field content was authored without accepted import material.

Mixed
    Field combines imported and operator-authored material.
```

## Advisory vs Canonical Visual Language

The visual system must distinguish advisory cognition from canonical lifecycle state.

Advisory source material should appear as:

- preview content
- source-linked
- clearly labeled non-canonical
- visually separate from lifecycle submission controls

Accepted draft material should appear as:

- editable thesis content
- provenance-marked
- draft-state content
- not event-backed until submission

Operator-authored modifications should appear as:

- editable text
- potentially marked as edited-after-import when derived from imported content
- owned by the operator in the draft

Canonical lifecycle state should appear only after explicit submission:

- event-backed
- lifecycle-owned
- replayable as canonical fact
- distinct from source advisory artifact

The UI should avoid language that implies automation or authority, such as "AI-created thesis" or "auto-promoted thesis." Preferred language should emphasize source, draft, review, acceptance, and operator submission.

## Operator Editing Behavior

Accepted imported material must remain editable.

Editing behavior should support:

- direct text editing after field acceptance
- retaining provenance marker after edit
- indicating that text has diverged from source
- removing accepted material from a draft field
- reverting to source text where practical
- adding operator-authored notes next to imported reasoning

Editing should shift cognition from imported advisory material toward operator-authored lifecycle intent. The operator's final submitted thesis is their authored decision artifact, even when it contains imported source-derived reasoning.

The system should not hide imported origins after editing. It should also not imply the source still exactly authored the final field after operator changes.

## Thesis Submission And Promotion Flow

Thesis submission is the explicit promotion point.

Promotion flow:

```text
Draft thesis fields
    -> operator reviews content and provenance
    -> operator submits Thesis workflow
    -> lifecycle validation applies
    -> canonical lifecycle event is appended
    -> promoted thesis references source advisory provenance
```

The submission action should communicate:

- this creates or updates lifecycle thesis state
- accepted advisory material remains source material
- the submitted thesis is operator-owned
- provenance will remain available for replay/review

Submission must use the normal Thesis workflow. The import system must not bypass lifecycle validation or event-sourcing paths.

## Replay Reconstruction Semantics

Replay should reconstruct both cognition source and lifecycle fact.

Replay should distinguish:

- imported advisory artifact existed
- operator previewed or used imported material where captured
- fields were accepted into draft where captured
- fields were edited after acceptance where captured
- thesis was submitted through normal lifecycle workflow
- canonical lifecycle event was appended

Replay should not treat imported content as historical lifecycle truth unless the operator promoted it.

Replay view should support questions such as:

- What source cognition influenced this thesis?
- Which fields came from imported research?
- Which fields were edited by the operator?
- What did the operator make canonical?
- What remained advisory and unused?

Open replay issue:

- The exact persistence level for field acceptance and edit history remains unresolved for `TF-R001`.

## Error And Rejection States

The workflow should handle import problems without blocking normal thesis authorship.

Possible states:

- no eligible import material
- unsupported artifact type
- schema mismatch
- missing symbol or ambiguous symbol
- stale artifact
- duplicate artifact
- parse or mapping failure
- rejected artifact
- rejected field
- deferred artifact

Behavior:

- Normal Thesis authoring remains available.
- Failed or unsupported imports remain advisory/import concerns, not lifecycle errors.
- Rejection removes material from the current workflow context but should not silently delete historical advisory artifacts.
- Deferred material remains available later if the substrate supports it.

Rejected imported material should not become canonical. Whether rejection itself is replay-visible remains an open question.

## Cross-Workspace UX Consistency Considerations

The Thesis import workflow should establish patterns reusable by Plan and Review imports.

Consistent concepts:

- global import preview
- lifecycle-local rendering
- selective acceptance
- editable draft incorporation
- provenance markers
- advisory/non-canonical visual distinction
- explicit lifecycle submission

Workspace-specific constraints:

- Thesis import focuses on reasoning, assumptions, catalysts, risks, and invalidation.
- Plan import must add stronger controls around sizing, approval, and execution authority.
- Review import must distinguish imported reflection from operator-authored review truth.

The Thesis workflow should avoid patterns that would become unsafe when later applied to Plan import.

## Open UX Questions

- Should the global import preview appear as a panel, drawer, modal, side rail, or workspace region?
- How prominent should import availability be when the operator is developing a thesis from an opportunity?
- Should field acceptance use explicit buttons, drag/drop, compare/merge, or inline actions?
- How should long imported narratives be summarized without hiding source material?
- What is the best visual treatment for edited-after-import fields?
- Should rejected fields disappear, collapse, or remain visible as rejected source material?
- Should the system show a pre-submit provenance review step?
- How much field-level history should be shown during authoring versus replay?
- Should `Previewed` become a durable state or remain session-local UX state?
- What minimum interaction trace is required for replay without over-instrumenting authoring?

## Recommended Thin UI Slice

The first UI slice should prove the boundary and authoring pattern.

Recommended thin slice:

```text
Opportunity Workspace
    -> Develop Thesis
    -> Thesis Workspace with import preview
    -> accept thesis narrative / catalysts / risks / invalidation
    -> edit accepted fields
    -> submit Thesis manually
    -> show source provenance after submission
```

Include:

- one eligible advisory artifact
- global preview
- field-level accept controls
- provenance marker on accepted fields
- edited-after-import marker
- reject/defer action
- explicit manual Thesis submission

Exclude:

- plan import
- watcher daemon
- background sync
- AI parsing
- automatic lifecycle event creation
- automatic conviction
- automatic sizing
- execution-adjacent controls

## Future Workflow Extensions

- Multiple import artifacts competing for the same thesis field.
- Compare/merge UX for existing draft text and imported text.
- Import provenance timeline inside Replay Workspace.
- Thesis revision import for later thesis evolution.
- Review Workspace import of external journal/reflection artifacts.
- Plan Workspace import with stricter execution-boundary controls.
- Candidate-to-thesis import patterns from advisory candidate queues.
- M15 replayable cognitive reconstruction using source cognition and accepted field history.
- M16 opportunity funnel workflows using import availability as attention context.

