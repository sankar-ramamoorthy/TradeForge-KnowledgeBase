---
title: M14 Behavioral Intelligence Synthesis
status: processed
type: implementation-synthesis
created: 2026-05-26
source:
  - knowledge/raw/20260526-m14-behavioral-intelligence-completion.md
tags:
  - TradeForge
  - M14
  - behavioral-signals
  - derived-read-model
  - review
---

# M14 Behavioral Intelligence Synthesis

M14 stabilizes behavioral intelligence as a deterministic review projection.
The runtime now supports cognitive auditability across impulsive execution,
process deviations, clustering, recurring mistakes, deterioration, thesis
attachment, emotional reflection context, behavior timelines, and
decision-quality metrics.

The semantic boundary is unchanged from ADR-0044:

- Behavioral outputs are derived read models.
- Source event and source signal references are mandatory for auditability.
- Behavioral analysis does not write event-ledger facts.
- Behavioral analysis does not alter lifecycle state, approval, execution, or
  human decision sovereignty.
- Emotional context must begin from operator-authored review text or explicitly
  accepted advisory artifacts; it must not become an inferred fact about the
  operator.

The reusable implementation pattern is:

```text
Event Ledger history
  -> deterministic behavioral signals
  -> higher-order derived review projections
  -> review/replay surfaces with authority labels
```

This pattern should be preserved for later cognitive auditability work unless a
future ADR intentionally introduces new review-event semantics.
