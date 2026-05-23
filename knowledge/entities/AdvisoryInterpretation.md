---
title: AdvisoryInterpretation
type: entity
status: canonical
tags: [TradeForge, entity, advisory, interpretation, cognition, M13]
created: 2026-05-20
updated: 2026-05-22
milestone: M13
runtime-issues: [TF-B001, TF-B002, TF-B003, TF-B004, TF-B005, TF-B006, TF-B007, TF-B008, TF-B009, TF-B010, TF-B011, TF-B012, TF-B013, TF-B014, TF-B015]
source_history:
  - knowledge/processed/20260521-m13-runtime-issue-spec-synthesis.md
  - knowledge/processed/20260522-m13-contextual-interpretation-closeout-synthesis.md
  - knowledge/processed/20260522-tf-b002-b009-interpretation-analytics-synthesis.md
  - knowledge/processed/20260522-tf-b010-b015-replay-ux-surfaces-synthesis.md
---

# AdvisoryInterpretation

## Definition

An AdvisoryInterpretation is a non-canonical advisory cognition artifact that
turns one or more AdvisoryObservations into contextual meaning.

It may describe contextual meaning, thesis influence, conflict, drift, or a
probabilistic summary, but it does not create or revise lifecycle state.

The M13 interpretation chain is:

```text
Observation -> Interpretation -> Contextual Weighting -> Thesis Influence
```

It is not:

```text
Indicator -> Buy/Sell
```

## Authority

Only the capture fact may enter the event ledger through
`advisory.interpretation_captured`.

The interpretation content and rationale remain advisory artifact content.

An AdvisoryInterpretation does not:

- create a TradeIdea
- revise a TradeThesis
- approve a TradePlan
- execute a trade
- issue buy/sell instructions
- override deterministic lifecycle rules

## Required Context

An AdvisoryInterpretation must preserve:

- interpretation identity
- artifact identity
- linked AdvisoryObservation IDs
- interpretation kind
- thesis influence
- contextual weight
- advisory confidence range
- provenance summary
- caveats
- persona and workspace context
- captured timestamp

Decision and thesis links are contextual references only.

## Related Concepts

- [[AdvisoryObservation]]
- [[AdvisoryArtifact]]
- [[ThesisInfluence]]
- [[ContextualWeighting]]
- [[ConflictingEvidence]]
- [[InterpretiveUncertainty]]

## Runtime Stabilization

M13 stabilized advisory interpretation analytics, replay-visible capture facts,
probabilistic cognition summaries, and contextual reasoning timelines.

These surfaces remain derived advisory views. They do not revise theses, approve
plans, execute trades, or create lifecycle authority.
