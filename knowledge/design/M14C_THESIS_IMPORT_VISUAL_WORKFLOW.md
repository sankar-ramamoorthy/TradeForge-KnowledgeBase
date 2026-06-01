---
title: M14C Thesis Import Visual Workflow
type: design
status: draft
created: 2026-05-27
updated: 2026-05-27
tags:
  - TradeForge
  - M14C
  - Thesis-Workspace
  - UX-workflow
  - visual-semantics
  - advisory-artifact
  - provenance
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

# M14C Thesis Import Visual Workflow

## Summary

This document defines visual UX and workflow concepts for Thesis Workspace import behavior in M14C.

The visual model must make imported research feel like advisory cognition, not automated lifecycle action. The operator reviews source material, selectively accepts reasoning into draft fields, edits it into authored lifecycle intent, and explicitly submits the Thesis workflow before canonical lifecycle state changes.

This document is not a frontend implementation spec. It defines workflow regions, visual semantics, state transitions, and replay-facing presentation concepts.

## Relationship To M14C Doctrine

This design implements the visual side of [[M14C Operator Cognition Bridge]] and [[M14C Thesis Import Workflow]].

Doctrinal constraints:

- Imported research artifacts remain advisory cognition.
- Accepted draft material is not canonical lifecycle truth.
- The Decision Lifecycle Engine remains lifecycle authority.
- The Event Ledger remains canonical truth.
- Human Decision Sovereignty remains mandatory.
- The UI must not imply automatic thesis creation, automatic plan creation, approval, execution authorization, conviction assignment, or sizing.

Visual design should reinforce the distinction between:

- advisory source material
- accepted draft material
- operator-authored modifications
- canonical lifecycle state

## Operator Cognitive Flow

The operator should experience the workflow as a deliberate reasoning path:

```text
Notice import availability
    -> inspect source cognition
    -> compare to opportunity context
    -> accept useful fields
    -> edit into operator-owned thesis reasoning
    -> review provenance
    -> submit Thesis explicitly
    -> inspect replay-visible source trail later
```

The UI should create cognitive pauses at the right places:

- before accepting imported content
- before replacing existing draft text
- before submitting the Thesis workflow
- when imported content has execution-adjacent or confidence-like language

The workflow should feel like structured reasoning assistance and advisory evidence review, not autocomplete trading.

## Annotated Opportunity -> Thesis Workflow

```text
+-----------------------+
| Opportunity Workspace |
+-----------------------+
          |
          | operator selects Develop Thesis
          v
+---------------------------+
| Thesis Workspace Opens    |
| lifecycle authoring mode  |
+---------------------------+
          |
          | eligible advisory artifact found
          v
+---------------------------+       +--------------------------+
| Thesis Draft Area         | <---- | Import Preview Panel     |
| operator-owned authoring  |       | advisory / non-canonical |
+---------------------------+       +--------------------------+
          |
          | operator accepts selected fields
          v
+---------------------------+
| Draft Fields With         |
| provenance indicators     |
+---------------------------+
          |
          | operator edits and submits
          v
+---------------------------+
| Canonical Thesis Event    |
| appended through normal   |
| lifecycle authority       |
+---------------------------+
```

Annotated boundaries:

- Opportunity context may suggest relevance, but does not create thesis state.
- Import preview shows advisory source material, not lifecycle content.
- Draft area belongs to the operator's authoring workflow.
- Submission is the only canonical promotion point.

## Thesis Workspace Layout Concepts

Conceptual regions:

```text
+------------------------------------------------------------------+
| Lifecycle Header: Opportunity -> Develop Thesis                  |
| active decision, symbol, persona, lifecycle stage, source status |
+-------------------------------+----------------------------------+
| Thesis Authoring Surface      | Import Preview / Source Cognition |
|                               |                                  |
| - title                       | - artifact summary               |
| - thesis narrative            | - source/provenance              |
| - catalysts                   | - mapped fields                  |
| - risks                       | - accept/reject/defer controls   |
| - assumptions                 | - caveats / uncertainty          |
| - invalidation                |                                  |
| - evidence links              |                                  |
+-------------------------------+----------------------------------+
| Provenance Review Strip / Submission Footer                       |
| accepted fields, edited fields, explicit Submit Thesis action     |
+------------------------------------------------------------------+
```

The import preview may be a panel, drawer, or workspace region. The key design requirement is semantic separation: import controls should not be visually merged with lifecycle submission controls.

## Import Preview Panel / Drawer Concepts

The import preview should answer:

- What is this source?
- Why is it relevant?
- What fields can it help author?
- What caveats or uncertainty came with it?
- What happens if I accept part of it?

Suggested preview structure:

```text
Import Preview
--------------
Status: Advisory source material
Source: Claude / Codex / Research Cockpit / operator note
Captured: timestamp
Artifact type: thesis_draft / research_note
Symbol: ATKR
Caveats: present / none recorded

Mapped Fields
-------------
[Preview] Thesis Narrative        [Accept]
[Preview] Catalysts               [Accept]
[Preview] Risks                   [Accept]
[Preview] Invalidation            [Accept]
[Preview] Evidence Links          [Accept]

Artifact Actions
----------------
[Defer] [Reject for this workflow] [View full source]
```

The preview must not show `Submit Thesis`, `Approve`, `Size`, or execution-like actions.

## Field Acceptance Interaction Patterns

Field acceptance should be deliberate and reversible before submission.

Basic pattern:

```text
Imported Field
    [Preview source text]
    [Accept into draft]
        -> Draft Field populated
        -> provenance marker attached
        -> field remains editable
```

Existing draft conflict pattern:

```text
Draft field already has text
    -> operator accepts imported field
    -> system asks:
        [Append]
        [Replace]
        [Compare / Merge]
        [Cancel]
```

Section acceptance pattern:

```text
Imported section: catalysts
    -> operator expands section
    -> individual catalyst rows can be accepted or rejected
    -> accepted rows appear in draft catalysts
```

The UI should prefer explicit verbs such as `Accept into draft`, `Use in draft`, or `Add to draft`. Avoid verbs that imply canonicalization, such as `Create Thesis` from the import panel.

## Accepted vs Edited Visualization

Accepted unchanged:

```text
Thesis Narrative
[source: imported research | unchanged]
"Aerospace recovery and defense spending..."
```

Accepted then edited:

```text
Thesis Narrative
[source: imported research | edited by operator]
"My thesis is that aerospace recovery..."
```

Operator-authored:

```text
Assumptions
[operator-authored]
"Market remains risk-on..."
```

Mixed:

```text
Risks
[mixed: imported + operator-authored]
- Imported: margin compression risk
- Operator note: watch debt refinancing schedule
```

Visual states should support scanning without treating provenance as the main content. The main cognitive surface remains the thesis draft.

## Provenance Badge Concepts

Provenance badges should be compact but inspectable.

Possible badge labels:

- `Advisory`
- `Imported`
- `Imported edited`
- `Operator-authored`
- `Mixed`
- `Source linked`

Badge detail popover or expansion should show:

```text
Source artifact: advisory-artifact-id
Origin: imported_research / claude_generated / codex_generated
Captured: timestamp
Source reference: file or tool reference
Caveats: ...
Accepted into field: timestamp or session marker, if captured
Edited after acceptance: yes/no
```

The badge should not imply that imported text is authoritative. Provenance explains origin, not truth.

## Advisory vs Canonical Visual Distinctions

Suggested semantic layers:

```text
Advisory Source
    preview panel, source labels, caveats, non-canonical wording

Draft Authoring
    editable fields, provenance markers, validation guidance

Promotion Action
    explicit Thesis submission, lifecycle language, operator confirmation

Canonical Result
    event-backed state after submission, replay-visible lifecycle fact
```

The UI should visually separate import controls from lifecycle controls.

Unsafe visual patterns:

- import panel with a primary `Create Thesis` action
- imported text displayed as if it already belongs to the thesis
- provenance hidden until after submission
- AI-generated material using the same visual treatment as operator-authored canonical state

## Replay Visualization Concepts

Replay should reconstruct both source cognition and canonical transition.

Replay timeline concept:

```text
Advisory Artifact Captured
    source: Research Cockpit / Claude / Codex
    status: non-canonical

Thesis Draft Used Advisory Source
    accepted fields: narrative, catalysts, invalidation
    edited fields: narrative

Thesis Created
    canonical lifecycle event
    provenance: source artifact references
```

Replay detail concept:

```text
Canonical Thesis
----------------
Operator-submitted thesis text

Source Cognition
----------------
Imported advisory artifact
Accepted fields
Edited-after-import indicators
Unused/rejected sections where captured
```

Replay must not show imported material as if it was canonical before submission.

## Compare / Merge Interaction Concepts

Compare/merge is optional for the first thin slice but important for workflow maturity.

Concept:

```text
+--------------------------+--------------------------+
| Current Draft            | Imported Advisory Field  |
| operator text            | source text              |
+--------------------------+--------------------------+
| [Keep draft] [Replace] [Append] [Merge manually]     |
+------------------------------------------------------+
```

Merge should remain operator-controlled. The system may display differences, but should not auto-resolve thesis meaning.

Open question:

- Should compare/merge be part of `TF-R001`, or deferred until multiple artifact and existing-draft scenarios are common?

## Error / Rejection Visual Flows

No eligible import:

```text
No imported advisory cognition available for this thesis.
Continue authoring normally.
```

Unsupported artifact:

```text
This advisory artifact cannot be mapped to thesis fields.
[View source] [Dismiss]
```

Ambiguous symbol:

```text
This artifact references multiple instruments.
Select applicable symbol before accepting fields.
```

Rejected field:

```text
Rejected for this thesis workflow.
[Undo rejection] [View source]
```

Design rule:

- import errors should not block Thesis authoring
- rejection should not delete advisory history
- rejected source material should remain distinguishable from accepted draft content

## Thin UI Slice Mockup Concepts

Minimum useful UI for `TF-R001`:

```text
+------------------------------------------------------------------+
| Develop Thesis: ATKR                                             |
| Lifecycle status: Idea -> Thesis                                 |
+-----------------------------------+------------------------------+
| Thesis Draft                      | Import Preview               |
|                                   | Status: Advisory             |
| Title [operator/draft]            | Source: imported research    |
| Narrative [Accept marker]         |                              |
| Catalysts [Accept marker]         | Thesis narrative [Accept]    |
| Risks [operator-authored]         | Catalysts [Accept]           |
| Invalidation [Accept marker]      | Risks [Accept]               |
| Evidence links                    | Invalidation [Accept]        |
+-----------------------------------+------------------------------+
| Provenance: 3 accepted, 1 edited                                  |
| [Submit Thesis]                                                   |
+------------------------------------------------------------------+
```

Thin slice interaction path:

```text
Open Develop Thesis
    -> inspect one imported artifact
    -> accept narrative, catalysts, invalidation
    -> edit narrative
    -> reject risks
    -> submit Thesis
    -> view post-submit provenance summary
```

## Open Visual UX Questions

- Should import preview be always visible, collapsible, or opened on demand?
- Should accepted-field badges be inline, field-header based, or grouped in a provenance strip?
- What is the clearest visual treatment for `Imported edited` without implying source text remains unchanged?
- Should rejected fields remain visible in the import panel after rejection?
- Should the pre-submit footer include a provenance review checklist?
- How should the UX handle multiple eligible advisory artifacts for the same symbol?
- Should compare/merge be included in `TF-R001` or deferred?
- How much provenance belongs in the authoring view versus replay view?

