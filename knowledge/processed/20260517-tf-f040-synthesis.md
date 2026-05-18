---
title: TF-F040 Synthesis - Trader Language Boundary
type: processed-synthesis
status: processed
tags: [TradeForge, TF-F, M10E, trader-language, ux]
created: 2026-05-17
updated: 2026-05-17
source_history:
  - knowledge/raw/brainstorm-20260517-opportunity-workspace-ux-and-context-acquisition.md
  - knowledge/raw/20260517-tf-f028-to-tf-f042-diagnosis.md
  - knowledge/raw/20260517-tf-f028-to-tf-f042-planning.md
  - knowledge/raw/20260517-tf-f040-planning.md
related:
  - "[[UX_DOCTRINE]]"
  - "[[Terminology Stability]]"
issues:
  - TF-F040
---

# TF-F040 Synthesis - Trader Language Boundary

## What Was Stabilized

TradeForge now explicitly distinguishes canonical internal semantics from
operator-facing language.

## Why It Was Needed

The Opportunity Workspace discovery session showed that correct internal terms
such as `candidate` and `scenario branches` can still degrade operator cognition
when used as primary UX language.

## What Changed

- Added the canonical `design/trader-language-doctrine.md`.
- Extended `UX_DOCTRINE.md` with a trader-language boundary section.
- Updated runtime `frontend/DESIGN.md` so UI implementation treats trader-native
  copy as a translation responsibility rather than an ad hoc styling choice.

## Architectural Meaning

This preserves glossary stability while making the UX layer responsible for
translation:

```text
canonical semantics -> operator interpretation -> clearer cognition
```

## ADR Evaluation

No ADR was required because the work clarified UX doctrine without changing
lifecycle semantics, event models, or bounded contexts.

## Follow-On Dependencies

- `TF-F029`
- `TF-F035`
- `TF-F039`
