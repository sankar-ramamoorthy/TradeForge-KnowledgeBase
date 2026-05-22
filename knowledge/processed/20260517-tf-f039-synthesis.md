---
title: TF-F039 Synthesis - Missing Information Doctrine
type: processed-synthesis
status: processed
tags: [TradeForge, TF-F, M10E, missing-information, ux]
created: 2026-05-17
updated: 2026-05-17
source_history:
  - knowledge/raw/brainstorm-20260517-opportunity-workspace-ux-and-context-acquisition.md
  - knowledge/raw/20260517-tf-f028-to-tf-f042-diagnosis.md
  - knowledge/raw/20260517-tf-f039-planning.md
related:
  - "[[UX_DOCTRINE]]"
issues:
  - TF-F039
---

# TF-F039 Synthesis - Missing Information Doctrine

## What Was Stabilized

TradeForge now has a recovery-oriented rule for missing-information states.

## What Changed

- Added canonical `design/missing-information-doctrine.md`.
- Extended `UX_DOCTRINE.md` with the recovery-oriented missing-information rule.
- Updated `frontend/DESIGN.md` with the runtime translation requirement.

## Architectural Meaning

Missing information is now treated as part of decision context rather than a
generic blank state. The doctrine distinguishes not-requested, loading, failed,
unsupported, not-configured, intentionally omitted, and stale states.

## ADR Evaluation

No ADR required; this clarified doctrine without changing system boundaries.

