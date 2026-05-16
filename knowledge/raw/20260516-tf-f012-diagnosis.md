---
title: TF-F012 Diagnosis - Workstation Layout Gap
type: raw-diagnosis
status: raw
tags: [TradeForge, TF-F012, feedback, frontend, workspace-layout, ux]
created: 2026-05-16
updated: 2026-05-16
source_history:
  - knowledge/raw/2026-05-16 feed back on screen real estate and design.md
  - knowledge/raw/feedback on screen realestate Screenshot 2026-05-16 120442.png
  - design/workspace-operational-layouts.md
  - design/workspace-zoning-reference.md
  - design/operational-panel-patterns.md
related:
  - "[[UX_DOCTRINE]]"
  - "[[WORKSPACES]]"
  - "[[INVARIANTS]]"
issues:
  - TF-F012
---

# TF-F012 Diagnosis - Workstation Layout Gap

## Observable Gap

The current desktop workspace presentation does not use available screen width as an operational resource.

Observed symptoms:

- large unused left and right margins on desktop
- narrow centered workspace shell
- active decisions compressed into stacked rows instead of side-by-side operational surfaces
- contextual awareness content placed inside the main flow instead of a persistent contextual zone
- the workspace reads as a centered application page rather than a workstation-oriented operational environment

## Evidence

Source feedback:

- `knowledge/raw/2026-05-16 feed back on screen real estate and design.md`
- `knowledge/raw/feedback on screen realestate Screenshot 2026-05-16 120442.png`

Runtime implementation evidence:

- `TradeForge/frontend/DESIGN.md` defines `shell_max_width: "1220px"`
- `TradeForge/frontend/src/styles.css` applies a centered `.app-shell`
- `TradeForge/frontend/src/operationalLayout.tsx` currently exposes a two-zone `sidebar + main` layout primitive

Design-direction evidence:

- `design/workspace-operational-layouts.md`
- `design/workspace-zoning-reference.md`
- `design/operational-panel-patterns.md`

## Root Cause

The frontend runtime translation layer still encodes an older centered-shell composition model that no longer matches the evolved TradeForge workspace doctrine.

The issue is not primarily color, typography, or spacing tokens.

The issue is:

```text
workspace composition mismatch
```

Current implementation behavior remains optimized for:

- centered content
- sequential reading
- constrained application shells

TradeForge doctrine requires:

- parallel cognition
- persistent situational context
- workspace-specific operational surfaces
- dense but readable use of screen width

## Relevant Invariants

- [[UX Is Architectural]]
- [[Workspaces Are Operational Environments]]
- [[Human Decision Sovereignty]]

## Architectural Interpretation

The gap sits in the projection / workspace UX layer.

It does not change:

- lifecycle semantics
- event taxonomy
- replay authority
- canonical truth boundaries

It does affect whether the UI correctly manifests existing workspace doctrine.

## Minimum Correct Scope

The minimum correct runtime change is:

1. remove centered-shell assumptions from desktop operational workspaces
2. introduce a desktop three-zone workspace composition model:
   - navigation zone
   - primary operational surface
   - contextual awareness rail
3. convert the Operating Workspace first as the proving surface
4. preserve explicit advisory versus canonical separation

## Explicitly Out Of Scope

- resizable panel systems
- detachable or user-rearrangeable workspaces
- multi-monitor features
- redesigning every workspace in the first pass
- mobile-first layout optimization beyond safe responsive degradation

## ADR Checkpoint

No ADR is currently required.

Reason:

- the issue remains a UX enhancement within existing doctrine
- it does not alter ontology, lifecycle semantics, event models, or invariant boundaries

Re-evaluate ADR need only if implementation formalizes a new durable runtime architecture rule that changes future workspace composition authority across the product.

## Diagnosis Conclusion

`TF-F012` is a bounded frontend enhancement caused by stale runtime layout translation, not by missing high-level UX doctrine.

The correct fix is to align runtime workspace composition with already-emerging operational workspace doctrine, starting with one reference workspace rather than redesigning the entire frontend at once.
