---
title: Evidence Density And Attention Ranking Synthesis
type: processed-synthesis
status: processed
created: 2026-07-12
updated: 2026-07-12
tags: [TradeForge, evidence-density, attention-ranking, market-context, roadmap]
source_history:
  - knowledge/raw/archived/20260712-M-RF-completed-and-next-is-EV.md
  - knowledge/raw/archived/brainstorm-20260712-evidence-density-attention-ranking.md
related:
  - "[[Evidence Density And Attention Ranking]]"
  - "[[OperationalAttentionQueue]]"
  - "[[CognitiveEvidence]]"
  - "[[MarketContext]]"
related_runtime_docs:
  - TradeForge/DOCS/ISSUE_REGISTER.md
  - TradeForge/DOCS/Milestone_Roadmap_v2.md
related_adrs:
  - ADR-0010
  - ADR-0013
  - ADR-0032
  - ADR-0038
  - ADR-0041
---

# Evidence Density And Attention Ranking Synthesis

## Processing Result

The raw source stabilizes a near-term product decision: after M-RF, TradeForge
should start with Evidence Density rather than paper execution. Paper trading
closes the execution loop only after the operator can already identify which
setup deserves attention. The current product gap is upstream:

- what deserves attention
- why it is interesting now
- what changed
- what evidence confirms or weakens it
- which opportunity should be examined first

The existing M-EZ roadmap already contains EV-01 through EV-04, but it lacks an
explicit authority step for evidence freshness, symbol eligibility, and
transparent attention ranking. That gap should be closed before EV-01 ships.

## Stabilized Direction

Add two planning units to M-EZ:

- EV-00 - Define Evidence Density And Attention Ranking Semantics
- EV-05 - Transparent Attention Ranking

The implementation sequence becomes:

```text
EV-00  design authority
EV-01  scheduled market snapshots
EV-02  lightweight watchlist
EV-05  transparent attention ranking
EV-03  per-symbol evidence panel
EV-04  basic chart
```

This keeps the first product-value slice focused on evidence density and
attention quality before paper execution.

## Semantic Conclusions

Evidence Density is not merely more market data. It is the minimum operational
evidence needed for TradeForge to answer why a symbol or decision deserves
operator attention now.

Attention ranking is derived state. It may order opportunities, watchlist
entries, and active decisions, but it is not canonical truth, lifecycle
authority, execution permission, or a buy/sell recommendation.

The first ranking model should be deterministic and transparent. It should show
its reasons, such as stale evidence, meaningful price change, unusual volume,
proximity to declared levels, earnings proximity, active decision review needs,
and operator-pinned priority.

## Boundary Constraints

- Provider data remains read-only, advisory, non-canonical, and provenance
  labeled.
- Replay uses persisted snapshots, historical evidence artifacts, and
  deterministic ranking inputs. Replay must not call live providers.
- Watchlist membership remains pre-lifecycle and must not create a TradeIdea
  without explicit operator promotion through the existing workflow.
- Ranking reasons must be visible to the operator.
- No opaque AI score is needed for the first slice.
- Ranking must not imply recommendation or execution authority.

## Runtime Synchronization

Runtime planning should be updated as follows:

- add EV-00 to the issue register and issue index
- add EV-05 to the issue register and issue index
- refine EV-01 acceptance criteria to depend on EV-00 definitions for eligible
  symbols, freshness, missing evidence, cadence, and replay behavior
- update M-EZ linked issues from EV-01 through EV-04 to EV-00 through EV-05
- update the recommended near-term sequence so Evidence Density precedes paper
  execution phases beyond already planned setup work

## Canonical Readiness

The concept is stable enough for a topic page because it ties together existing
canonical concepts: [[OperationalAttentionQueue]], [[CognitiveEvidence]], and
[[MarketContext]]. It is not yet mature enough to redefine those entities or
add a new canonical entity page. EV-00 should decide whether runtime needs a
distinct evidence-priority read model or whether ranking belongs entirely inside
the existing operational attention projection family.

## Contradictions

No contradiction was found with the loaded invariants, architecture doctrine,
event taxonomy, provider-boundary ADRs, advisory-evidence ADRs, or operational
attention doctrine.

The only operational drift identified is roadmap under-specification: M-EZ has
evidence collection and presentation issues, but ranking authority and
freshness semantics were not explicit enough.
