---
title: Evidence Density And Attention Ranking Direction
type: brainstorm
status: raw
tags: [trading-system, brainstorm, evidence-density, attention-ranking, roadmap]
created: 2026-07-12
---

# Evidence Density And Attention Ranking Direction

## Trigger

The M-RF API boundary decomposition is complete, and the next product-value
activity needs to be selected from the current roadmap. The source note argues
that Evidence Density should precede paper trading because TradeForge still
does not sufficiently answer which opportunity deserves attention and why.

## Raw Input

The next milestone should be **Evidence Density**, not paper trading yet.

Paper trading helps after you already know which trade to pursue. The more
immediate problem remains:

```text
What deserves attention?
Why is it interesting now?
What changed?
What evidence confirms or weakens it?
Which opportunity should I examine first?
```

TradeForge currently structures those questions well but does not supply enough
answers.

Before coding EV-01, promote the blue-pin diagnosis into a short authoritative
design document covering:

```text
Evidence acquisition
Evidence freshness
Watchlist membership
Attention ranking
Per-symbol evidence
Operator-visible provenance
```

This matters because the current EV plan covers snapshots and panels, but
ranking is still under-specified. More automatically collected data could
otherwise produce a larger unranked list.

Then execute:

1. EV-01 - Scheduled market snapshots
2. EV-02 - Lightweight watchlist
3. Add explicit attention ranking, perhaps EV-02A or EV-05
4. EV-03 - Evidence panel
5. EV-04 - Basic chart

For now, defer M-RF2 dependency injection, frontend API client decomposition,
operator identity, first-run master-key wizard, broader family onboarding,
M15/M17/M18, live execution, and elaborate AI analytics.

Paper trading should follow the first evidence-density vertical slice. At that
point TradeForge will help select and understand a setup, and paper execution
can close the remainder of the lifecycle.

The immediate sequence should be:

```text
1. Promote the blue-pin evidence questions into an authoritative design note
2. Add explicit ranking/attention semantics to the roadmap
3. Authorize EV-01 - scheduled market snapshots
4. Follow with EV-02 - lightweight watchlist
5. Then build the per-symbol evidence panel
```

The reason to insert the ranking design before EV-01 is simple: TradeForge's
problem is not merely "not enough data." It is also that the Operating screen
lists without ranking. More snapshots without prioritization could create a
larger pile of evidence without improving attention.

The first implementation milestone should be Evidence Density, then paper
trading.

The next move should be a design/authority issue, not implementation yet.

Use this sequence:

```text
1. Create a canonical Evidence Density and Attention Ranking design note
2. Register a new issue for transparent attention ranking
3. Refine EV-01 acceptance criteria against that design
4. Authorize EV-01
```

A suitable issue could be:

```text
EV-00 - Define Evidence Density And Attention Ranking Semantics
```

Its job is to define:

- which symbols are eligible for evidence refresh
- what "fresh," "stale," and "missing" mean
- what facts belong in the first evidence slice
- how attention priority is calculated
- how ranking reasons are exposed
- what remains advisory versus canonical
- how replay avoids live-data contamination

Then the implementation order becomes:

```text
EV-00  design authority
EV-01  scheduled market snapshots
EV-02  lightweight watchlist
EV-05  transparent attention ranking
EV-03  per-symbol evidence panel
EV-04  basic chart
```

The key guardrail is:

> TradeForge may rank what deserves attention, but it must always show the
> reasons and must not present ranking as a buy/sell recommendation.

## Observations

- The existing EV plan has snapshot, watchlist, panel, and chart issues, but
  does not define transparent attention-ranking semantics as its own step.
- Evidence acquisition without prioritization risks increasing cognitive load.
- The first product-value work after M-RF should answer attention questions
  before execution questions.
- Ranking must be deterministic, explainable, provenance-backed, and clearly
  non-authoritative.

## Ideas

- Add EV-00 as a design authority issue before EV-01.
- Add EV-05 as the implementation issue for transparent attention ranking.
- Refine EV-01 so snapshot eligibility and freshness are governed by EV-00.
- Keep watchlist membership pre-lifecycle and distinct from TradeIdea creation.
- Build evidence panels only after snapshots, watchlist membership, and ranking
  semantics exist.

## Questions

- Should EV-00 produce only runtime documentation, or should it also promote a
  KB topic/entity for Evidence Density?
- Should attention priority become part of the existing Operational Attention
  Queue projection, or start as a distinct evidence-priority read model?
- Which ranking factors should ship in the first deterministic slice?

## Concerns

- Ranking must not become an opaque AI score.
- Ranking must not imply buy/sell recommendation authority.
- Watchlist CRUD must not create lifecycle events or silently promote symbols
  into TradeIdeas.
- Replay must use persisted snapshots and historical ranking inputs, never live
  provider calls.
- Evidence surfaces must keep provider provenance and freshness visible.

## Possible Next Outputs

- Processed synthesis
- Topic page update
- Runtime issue-register update
- Runtime roadmap update
- Raw archive
