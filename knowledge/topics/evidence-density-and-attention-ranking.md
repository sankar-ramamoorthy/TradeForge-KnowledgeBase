---
title: Evidence Density And Attention Ranking
type: topic
status: current
created: 2026-07-12
updated: 2026-07-12
tags: [TradeForge, evidence-density, attention-ranking, market-context, operational-attention]
source_history:
  - knowledge/processed/20260712-evidence-density-attention-ranking-synthesis.md
related:
  - "[[OperationalAttentionQueue]]"
  - "[[CognitiveEvidence]]"
  - "[[MarketContext]]"
---

# Evidence Density And Attention Ranking

## Definition

Evidence Density is the product capability that supplies enough timely,
source-linked market and decision context for an operator to understand what
deserves attention and why.

Attention Ranking is the deterministic derived ordering of evidence-bearing
symbols, watchlist entries, and active decisions according to explicit,
operator-visible reasons.

## Semantic Role

Evidence Density answers:

- what changed
- why it may matter
- what confirms or weakens the current idea
- what remains missing
- which opportunity should be examined first

Attention Ranking prevents evidence collection from becoming an unprioritized
pile of market data.

The first runtime slice landed on 2026-07-12 as EV-00 through EV-05.

## Authority Boundary

Evidence and ranking are advisory and derived. They are not canonical truth,
lifecycle authority, execution permission, or buy/sell recommendations.

Ranking may influence what appears first in an operational surface, but it must
not approve, reject, promote, execute, or mutate lifecycle state.

## First-Slice Ranking Reasons

The first transparent ranking model uses:

- stale evidence
- meaningful price change
- unusual volume
- active decision requiring review
- operator-pinned priority
- watchlist monitoring
- missing evidence

Each ranked item should expose the reasons that contributed to placement.

## Replay Requirement

Replay must reconstruct evidence and ranking from historical snapshots,
historical advisory artifacts, event-backed decision state, and deterministic
rules. It must not call live providers or current AI systems.

## Implementation Sequence

```text
EV-00  design authority
EV-01  scheduled market snapshots
EV-02  lightweight watchlist
EV-05  transparent attention ranking
EV-03  per-symbol evidence panel
EV-04  basic chart
```

## Runtime Implementation

- Runtime authority note:
  `TradeForge/DOCS/evidence-density-and-attention-ranking.md`
- Canonical watchlist intent:
  `market.watchlist_entry_added`, `market.watchlist_entry_updated`
- Advisory endpoints:
  `/evidence/watchlist`, `/evidence/eligibility`, `/evidence/refresh/run`,
  `/evidence/ranking`, `/evidence/symbols/{symbol}`
- Workspace surfaces:
  Operating Workspace evidence attention ranking; Opportunity Workspace
  per-symbol evidence panel and basic persisted-snapshot chart.

## Related Concepts

- [[OperationalAttentionQueue]]
- [[CognitiveEvidence]]
- [[MarketContext]]
