---
title: TF-F012 / TF-F013 Screen Layout Planning Synthesis
type: processed-synthesis
status: processed
tags: [TradeForge, TF-F, feedback, workspace-layout, operational-cognition, design-doctrine, frontend]
created: 2026-05-16
updated: 2026-05-16
source_history:
  - knowledge/raw/plan for TF-0012 thru TF-0014 for Screen layout 20260516.md
related:
  - "[[UX_DOCTRINE]]"
  - "[[WORKSPACES]]"
  - "[[INVARIANTS]]"
  - "[[DESIGN.md Role And Boundary]]"
  - "[[Behavioral And Experiential Architecture]]"
  - "[[Workspace Operational Layouts]]"
  - "[[Workspace Zoning Reference]]"
  - "[[Operational Panel Patterns]]"
issues:
  - TF-F012
  - TF-F013
---

# TF-F012 / TF-F013 Screen Layout Planning Synthesis

## Context

This synthesis processes a planning capture derived from the 2026-05-16 screen-real-estate feedback review.

The source note consolidates the interpretation that the observed frontend problem is not a token-system defect, but a mismatch between:

- TradeForge's workspace and UX doctrine
- the newer draft operational-layout documents
- the current frontend runtime translation layer

The source planning note was archived after this synthesis:

- `knowledge/raw/archived/plan for TF-0012 thru TF-0014 for Screen layout 20260516.md`

## Core Synthesis

The feedback should currently be treated as two distinct but related feedback issues:

1. `TF-F012` should own the runtime-facing workspace layout gap.
2. `TF-F013` should own the supporting design-doctrine clarification gap.

This division preserves scope discipline:

- `TF-F012` addresses the operational symptom and runtime cause.
- `TF-F013` addresses the documentation-layer ambiguity that allowed the mismatch to emerge.

The two issues should remain linked, but they should not collapse into one mixed implementation/doctrine ticket.

## TF-F012 Stabilized Interpretation

### Issue Shape

`TF-F012` is the primary issue.

Recommended title:

```text
Replace centered workspace shell with workstation-oriented operational layout model
```

Recommended classification:

```text
enhancement
```

### Observable Gap

Current desktop workspaces:

- underuse available horizontal screen space
- retain large dead margins around the active workspace
- compress active decisions into stacked vertical sections
- lack a persistent contextual awareness rail
- behave more like centered application pages than persona-scoped operational workspaces

### Diagnosed Cause

The runtime frontend translation layer still reflects an older centered-shell layout model:

- `frontend/DESIGN.md` defines a fixed `shell_max_width`
- `frontend/src/styles.css` constrains `.app-shell` to a centered max-width shell
- shared layout primitives currently support `sidebar + main`, not the newer multi-zone workstation model

This conflicts with the workspace cognition direction already expressed in:

- [[UX_DOCTRINE]]
- [[WORKSPACES]]
- `design/workspace-operational-layouts.md`
- `design/workspace-zoning-reference.md`
- `design/operational-panel-patterns.md`

### Minimum Correct Scope

The first bounded implementation should:

- remove centered-shell assumptions for operational desktop workspaces
- introduce a desktop three-zone composition model:
  - navigation zone
  - primary operational surface
  - contextual awareness rail
- convert the Operating Workspace first as the proving surface
- preserve canonical versus advisory visual distinction

### Explicitly Out Of Scope

For the initial issue:

- resizable panels
- detachable panels
- multi-monitor support
- full redesign of every workspace in one pass
- mobile-first optimization beyond safe responsive degradation

### ADR Implication

No ADR appears mandatory at this stage if the work remains an enhancement within existing workspace and UX doctrine.

An ADR should be reconsidered only if implementation formalizes a new durable runtime architecture rule that changes future workspace composition authority across the product.

## TF-F013 Stabilized Interpretation

### Issue Shape

`TF-F013` is the supporting doctrine issue.

Recommended title:

```text
Formalize the three-layer design architecture between doctrine, workspace composition, and frontend translation
```

Recommended classification:

```text
doctrine
```

### Observable Gap

Current design documentation still partially reflects a two-layer model:

```text
design doctrine
    -> frontend implementation translation
```

The newer design work has exposed a necessary middle layer:

```text
design doctrine
    -> operational workspace architecture
    -> frontend implementation translation
```

### Diagnosed Cause

The system's design model evolved after the earlier processed understanding of `DESIGN.md`, but the explanatory documentation has not yet been normalized around that new middle layer.

This leaves room for frontend implementation to stay visually coherent while drifting from the intended workspace composition model.

### Minimum Correct Scope

The doctrine follow-up should:

- update the frontend-design role note with the three-layer model
- clarify that frontend translation does not own workspace meaning or operational composition doctrine
- decide whether the draft workspace-layout documents remain draft artifacts or enter an explicit promotion path

### Explicitly Out Of Scope

- runtime code changes
- immediate canonicalization of every draft design document
- broad redesign of UX doctrine

### ADR Implication

No runtime ADR appears required unless the three-layer model is elevated into a new future-facing architectural authority rule.

## Semantic And Ontology Observations

No new canonical entity is introduced by the planning capture.

The note reinforces existing semantic distinctions:

- Persona Workspaces are operational environments, not pages.
- UX is architectural, not cosmetic.
- frontend design tokens are translation artifacts, not doctrine owners.
- operational workspace composition is a distinct design concern from both cognitive doctrine and CSS primitives.

The note does not justify new glossary entries yet.

## Workflow And Playbook Implications

The planning capture confirms that the [[Feedback Issue Development Loop]] is the correct workflow:

- `TF-F012` should proceed through diagnosis before implementation.
- `TF-F013` should be handled as a bounded doctrine issue rather than absorbed silently into runtime implementation.

No playbook rewrite is required from this note alone.

## Operational Synchronization Findings

The raw note filename is inconsistent with its content:

- filename: `plan for TF-0012 thru TF-0014 for Screen layout 20260516.md`
- actual issue discussion: `TF-F012` and `TF-F013`

There is no stabilized `TF-0012`, `TF-0013`, or `TF-0014` screen-layout plan in the note body, and no `TF-F014` is defined.

This appears to be a naming artifact rather than a semantic expansion. Future issue registration should use the stabilized feedback identifiers:

- `TF-F012`
- `TF-F013`

## Promotion Assessment

Appropriate current promotion level:

```text
processed synthesis
```

Reason:

- the note clarifies issue structure and bounded scope
- it does not introduce a mature new ontology concept
- the related layout documents are still draft
- the doctrine relationship is stabilizing but not yet ready for canonical promotion

No topic-page or entity-page promotion is required from this raw note alone.

## Recommended Next Actions

1. Register `TF-F012` and `TF-F013` in the runtime issue register when they are ready to enter execution.
2. Create issue-specific diagnosis captures before any implementation work:
   - `knowledge/raw/20260516-tf-f012-diagnosis.md`
   - `knowledge/raw/20260516-tf-f013-diagnosis.md`
3. Keep `TF-F012` as the main runtime issue and `TF-F013` as the supporting doctrine issue.
4. Use the newer workspace-layout drafts as evidence and design input, but do not treat them as canonical doctrine until promoted deliberately.

## Contradictions

No contradiction with current canonical doctrine was identified.

The only inconsistency is operational naming drift in the source filename, which has been preserved above rather than silently normalized away.

## Processing Result

Stable knowledge was promoted to:

- `knowledge/processed/20260516-tf-f012-tf-f013-screen-layout-planning-synthesis.md`

The source raw note was archived after traceable promotion:

- `knowledge/raw/archived/plan for TF-0012 thru TF-0014 for Screen layout 20260516.md`
