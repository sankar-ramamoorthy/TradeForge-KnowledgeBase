---
title: Contextual Weighting
type: topic
status: canonical
tags: [TradeForge, advisory, interpretation, weighting, M13]
created: 2026-05-20
updated: 2026-05-20
runtime-issues: [TF-B002, TF-B003]
source_history:
  - knowledge/processed/20260522-tf-b002-b009-interpretation-analytics-synthesis.md
---

# Contextual Weighting

Contextual weighting is qualitative advisory metadata used to express how much
attention an interpretation deserves in its current context.

M13 uses low, medium, high, and watch. These are not numeric scores,
probabilities, rankings, or trade recommendations.

Weighting may consider regime sensitivity, provenance quality, uncertainty,
recency, and conflict, but it remains advisory interpretation rather than
canonical truth.

## Runtime Stabilization

M13 implemented contextual weight distribution and regime-aware weight
suggestion as advisory read-model surfaces.

Suggested weight is operator context, not an automatic mutation of an
AdvisoryInterpretation and not lifecycle authority.
