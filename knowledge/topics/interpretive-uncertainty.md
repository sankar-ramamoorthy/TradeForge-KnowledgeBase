---
title: Interpretive Uncertainty
type: topic
status: canonical
tags: [TradeForge, advisory, uncertainty, cognition, M13]
created: 2026-05-20
updated: 2026-05-20
runtime-issues: [TF-B005, TF-B012, TF-B013]
source_history:
  - knowledge/processed/20260522-tf-b002-b009-interpretation-analytics-synthesis.md
  - knowledge/processed/20260522-tf-b010-b015-replay-ux-surfaces-synthesis.md
---

# Interpretive Uncertainty

Interpretive uncertainty preserves the limits of advisory meaning.

M13 uses qualitative confidence ranges: low, medium, high, and unknown. These
ranges describe confidence in the interpretation artifact, not confidence that a
trade will succeed.

Uncertainty must remain visible in API and workspace surfaces.

## Runtime Stabilization

M13 implemented confidence-range distribution and probabilistic cognition
summary surfaces.

These surfaces preserve qualitative uncertainty. They must not be translated
into hidden predictive scores or recommendation authority.
