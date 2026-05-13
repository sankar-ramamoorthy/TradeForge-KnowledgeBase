---
title: Implementation — TF-0048 Market Regime Interpretation Model
type: processed-knowledge
status: processed
source: knowledge/raw/20260513-tf-0048-market-regime-interpreter-implementation.md
issue: TF-0048
milestone: M9
created: 2026-05-13
---

# TF-0048 Implementation: Market Regime Interpretation Model

## Outcome

`SingleBarRegimeInterpreter` implemented and verified. Protocol in domain follows
`MarketDataProvider` pattern. 19 tests passing, 359 total. All checks clean.

## Canonical Regime Rules

```
(H - L) / O > 4%   → HIGH_VOLATILITY  (priority 1)
(H - L) / O < 0.5% → LOW_VOLATILITY   (priority 2)
(C - O) / O > 2%   → BULL             (priority 3)
(C - O) / O < -2%  → BEAR             (priority 4)
otherwise          → RANGING           (priority 5)
zero O or error    → UNKNOWN           (fallback)
```

Thresholds are strict inequalities. "Exactly at threshold" falls to the next rule.

## Architectural Pattern Confirmed

Protocol in domain (boundary), implementation in services (deterministic logic),
wired at composition root (application.py). This is the same pattern as
`MarketDataProvider` / adapters. The regime interpreter can be swapped without
touching domain or services consumer code.

## Advisory Boundary Preservation

- Regime is always INFERRED, never canonical
- Classification captures "today's bar character", not multi-week market regime
- Zero-open and exception cases return UNKNOWN rather than guessing
- Frontend shows regime badge only when not UNKNOWN (avoids misleading display)

## Next: TF-0049

Contextual operational summaries combine workspace projection state + regime
classification + market context into a unified advisory narrative per workspace.
The regime field is now populated and ready to consume.
