---
title: AdvisoryInterpretation
type: entity
status: canonical
tags: [TradeForge, entity, advisory, interpretation, cognition, M13]
created: 2026-05-20
updated: 2026-05-20
milestone: M13
runtime-issues: [TF-B001, TF-B010, TF-B014, TF-B015]
---

# AdvisoryInterpretation

## Definition

An AdvisoryInterpretation is a non-canonical advisory cognition artifact that
turns one or more AdvisoryObservations into contextual meaning.

It may describe contextual meaning, thesis influence, conflict, drift, or a
probabilistic summary, but it does not create or revise lifecycle state.

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
- [[ThesisInfluence]]
- [[ContextualWeighting]]
- [[ConflictingEvidence]]
- [[InterpretiveUncertainty]]
