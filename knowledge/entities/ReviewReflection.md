---
title: ReviewReflection
type: entity
status: canonical
tags: [TradeForge, entity, review, m10a, cognitive-artifact]
created: 2026-05-14
updated: 2026-05-14
aliases: [Review Artifact, Post-Decision Reflection]
milestone: M10A
kb-issue: KB-M10A-004
---

# ReviewReflection

## Definition

A ReviewReflection is a structured post-decision learning artifact created during the Review
lifecycle stage.

It captures the operator's retrospective analysis of both decision quality and execution quality,
separate from outcome quality.

## Fields (Planned — M10AIS11)

| Field | Type | Description |
|---|---|---|
| thesis_vs_outcome | str | comparison of original thesis against what actually happened |
| decision_quality | int (1–5) | quality of the reasoning process regardless of outcome |
| execution_quality | int (1–5) | quality of execution against the stated plan |
| discipline_observations | str | notes on discipline adherence or deviation |
| lessons_learned | list[str] | reusable learning artifacts from this decision |
| behavioral_observations | str | patterns or tendencies the operator noticed |

## Semantic Distinction

ReviewReflection separates:
- decision quality (was the reasoning process sound?)
- execution quality (was the plan followed?)
- outcome quality (did the market go the expected direction?)

A good decision can produce a bad outcome.
A bad decision can produce a good outcome.
ReviewReflection preserves this distinction for accurate behavioral learning.

## Authority Boundary

ReviewReflection is a learning artifact.

It does not:
- modify historical lifecycle state
- authorize future decisions
- bypass lifecycle authority

## Replay Significance

ReviewReflections allow the review workspace to compare:
- original thesis (from StructuredThesis) against what actually happened
- plan rationale against actual execution
- behavioral patterns across multiple decisions

## Related Concepts

- [[StructuredThesis]]
- [[StructuredTradePlan]]
- [[ReviewArtifact]]
- [[CognitiveSnapshot]]
- [[DecisionArtifact]]
