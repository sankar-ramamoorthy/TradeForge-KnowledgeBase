
## Recommended next product direction

The next milestone should be **Evidence Density**, not paper trading yet.

Paper trading helps after you already know which trade to pursue. Your more immediate problem remains:

```text
What deserves attention?
Why is it interesting now?
What changed?
What evidence confirms or weakens it?
Which opportunity should I examine first?
```

TradeForge currently structures those questions well but does not supply enough answers.

## First next issue

Before coding EV-01, promote the blue-pin diagnosis into a short authoritative design document covering:

```text
Evidence acquisition
Evidence freshness
Watchlist membership
Attention ranking
Per-symbol evidence
Operator-visible provenance
```

This matters because the current EV plan covers snapshots and panels, but **ranking is still under-specified**. More automatically collected data could otherwise produce a larger unranked list.

Then execute:

### 1. EV-01 — Scheduled market snapshots

Start with:

* symbols attached to active decisions
* configurable refresh interval
* persisted snapshots
* explicit timestamps and provider provenance
* no live calls during replay
* no autonomous lifecycle changes

Keep the first implementation simple. It does not need sophisticated market-hours scheduling immediately; a deterministic periodic job is enough for the first slice.

### 2. EV-02 — Lightweight watchlist

A watchlist should remain pre-lifecycle:

```text
symbol
why I am watching
date added
status
```

It should not require a thesis or create a TradeIdea.

### 3. Add explicit attention ranking

This is missing from the existing roadmap and should be added as its own issue, perhaps `EV-02A` or `EV-05`.

The first ranking can be deterministic and transparent:

* stale evidence
* meaningful price change
* unusual volume
* proximity to a declared level
* earnings proximity
* active decision requiring review
* operator-pinned priority

The UI should explain why an item ranked where it did. No opaque AI score is necessary.

### 4. EV-03 — Evidence panel

Only after snapshots and watchlist data exist should the UI answer:

* what changed
* why it may matter
* what confirms
* what invalidates
* what remains missing

### 5. EV-04 — Basic chart

A chart belongs inside the decision/evidence surface, not as a dashboard replacement.

## What should wait

For now, defer:

* M-RF2 dependency injection
* frontend API client decomposition
* operator identity
* first-run master-key wizard
* broader family onboarding
* M15/M17/M18
* live execution
* elaborate AI analytics

Paper trading should follow the first evidence-density vertical slice. At that point TradeForge will help select and understand a setup, and paper execution can close the remainder of the lifecycle.

Good — then the refactor phase is fully behind you.

The next step is to choose the first **product-value** issue, not another cleanup milestone.

My recommendation remains:

**Start with Evidence Density, beginning with the missing design authority for evidence + ranking, then authorize EV-01.**

The immediate sequence should be:

```text
1. Promote the blue-pin evidence questions into an authoritative design note
2. Add explicit ranking/attention semantics to the roadmap
3. Authorize EV-01 — scheduled market snapshots
4. Follow with EV-02 — lightweight watchlist
5. Then build the per-symbol evidence panel
```

The reason to insert the ranking design before EV-01 is simple: TradeForge’s problem is not merely “not enough data.” It is also that the Operating screen **lists without ranking**. More snapshots without prioritization could create a larger pile of evidence without improving attention.

So the next concrete decision is:

> Do you want the first implementation milestone to be **Evidence Density** or **Paper Trading**?

My vote is **Evidence Density first**, then paper trading.


Good. The next move should be a **design/authority issue**, not implementation yet.

Use this sequence:

```text
1. Create a canonical Evidence Density and Attention Ranking design note
2. Register a new issue for transparent attention ranking
3. Refine EV-01 acceptance criteria against that design
4. Authorize EV-01
```

A suitable issue could be:

```text
EV-00 — Define Evidence Density And Attention Ranking Semantics
```

Its job is to define:

* which symbols are eligible for evidence refresh
* what “fresh,” “stale,” and “missing” mean
* what facts belong in the first evidence slice
* how attention priority is calculated
* how ranking reasons are exposed
* what remains advisory versus canonical
* how replay avoids live-data contamination

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

> TradeForge may rank what deserves attention, but it must always show the reasons and must not present ranking as a buy/sell recommendation.
