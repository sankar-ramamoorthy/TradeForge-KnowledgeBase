---
title: TF-F013 Diagnosis - Three-Layer Design Boundary
type: raw-diagnosis
status: raw
tags: [TradeForge, TF-F013, feedback, design-doctrine, frontend, semantic-governance]
created: 2026-05-16
updated: 2026-05-16
source_history:
  - knowledge/raw/20260515-frontend-design-md-role-and-usage.md
  - knowledge/raw/recomnended section addition to 20260515-frontend-design-md-role-and-usage.md
  - design/workspace-operational-layouts.md
  - design/workspace-zoning-reference.md
  - design/operational-panel-patterns.md
related:
  - "[[DESIGN.md Role And Boundary]]"
  - "[[Behavioral And Experiential Architecture]]"
  - "[[UX_DOCTRINE]]"
  - "[[WORKSPACES]]"
issues:
  - TF-F013
---

# TF-F013 Diagnosis - Three-Layer Design Boundary

## Observable Gap

TradeForge now effectively has three design layers, but explanatory documentation still partially reflects the older two-layer model.

Current stable/emerging layers:

```text
cognitive doctrine
    -> operational workspace architecture
    -> frontend implementation translation
```

Existing explanatory material still tends to describe:

```text
design doctrine
    -> frontend translation
```

## Evidence

- `knowledge/raw/20260515-frontend-design-md-role-and-usage.md`
- `knowledge/raw/recomnended section addition to 20260515-frontend-design-md-role-and-usage.md`
- `design/workspace-operational-layouts.md`
- `design/workspace-zoning-reference.md`
- `design/operational-panel-patterns.md`
- `knowledge/processed/design-md-role-and-boundary.md`

## Root Cause

The design model evolved after the earlier `DESIGN.md` boundary understanding was captured.

The missing middle layer has now emerged:

```text
operational workspace architecture
```

Without documenting that layer explicitly, frontend work can remain visually consistent while still drifting from the intended composition model.

## Relevant Invariants

- [[UX Is Architectural]]
- [[Terminology Stability]]
- [[Architectural Simplicity]]

## Architectural Interpretation

This is a doctrine/documentation boundary issue, not a runtime layout issue.

The missing clarity affects:

- semantic ownership
- future frontend interpretation
- promotion path for the draft workspace design documents

It does not itself alter:

- lifecycle behavior
- event models
- runtime authority boundaries

## Minimum Correct Scope

The minimum correct doctrine change is:

1. update the frontend-design role note with the explicit three-layer model
2. clarify that frontend translation is not canonical owner of workspace meaning or operational composition doctrine
3. state the relationship among:
   - `design/DESIGN_ARCHITECTURE.md`
   - operational workspace design drafts
   - `TradeForge/frontend/DESIGN.md`
4. preserve the draft status of newer design artifacts unless separately promoted

## Explicitly Out Of Scope

- runtime code changes
- immediate canonicalization of all workspace design drafts
- broad doctrine rewrite
- new glossary entries unless terminology stabilizes further

## ADR Checkpoint

No ADR is currently required.

Reason:

- this is a doctrine clarification within the KB / documentation layer
- no runtime architecture contract changes are being introduced yet

Re-evaluate only if the three-layer model is later promoted into a new durable architecture rule that changes authority relationships across runtime docs and implementation.

## Diagnosis Conclusion

`TF-F013` is the documentation counterpart to `TF-F012`.

It exists to prevent future drift by making the now-visible middle design layer explicit before more frontend implementation proceeds.
