---
title: TF-B010 Through TF-B015 Replay And UX Surfaces Synthesis
type: processed-synthesis
status: processed
created: 2026-05-22
updated: 2026-05-22
source_history:
  - knowledge/raw/archived/20260522-tf-b010-b015-replay-ux-surfaces-implementation.md
  - knowledge/raw/archived/20260522-m13-completion-summary.md
tags: [TradeForge, M13, advisory, replay, UX, interpretation]
related:
  - "[[AdvisoryInterpretation]]"
  - "[[InterpretiveUncertainty]]"
  - "[[ReplaySession]]"
---

# TF-B010 Through TF-B015 Replay And UX Surfaces Synthesis

## Stabilized Outcome

TF-B010 through TF-B015 made advisory interpretation visible to replay and API
surfaces while preserving the difference between canonical events and advisory
content.

## Replay Semantics

`advisory.interpretation_captured` events appear in replay timelines as advisory
entries. Timeline entries include capture metadata but exclude interpretation
content and rationale.

This preserves the event taxonomy rule:

```text
events record facts
interpretations remain advisory content
```

## Interpretation-First Surface Semantics

Interpretation API responses now carry the fields needed by workspace surfaces:

- interpretation narrative
- caveats
- provenance summary
- confidence range
- thesis influence
- contextual weight
- advisory authority label
- non-canonical label

The backend does not expose execute, approve, buy/sell, or lifecycle-transition
controls through these advisory surfaces.

## Uncertainty Preservation

M13 reinforces qualitative uncertainty:

- observations preserve uncertainty bands
- interpretations preserve confidence ranges
- generated responses preserve caveats, confidence, and operator acceptance
  requirements

No numeric score should be treated as a hidden recommendation authority.

## Reasoning Timeline

The contextual reasoning timeline composes replay timeline advisory entries with
interpretation analytics for a decision or thesis.

This is a derived review surface. It supports cognitive reconstruction but does
not become canonical truth.

## Frontend Follow-On

Frontend panels should render confidence range, uncertainty, caveats, and
advisory labels visibly. This is a UX translation task, not a change to
semantic authority.
