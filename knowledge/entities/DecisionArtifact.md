---
title: DecisionArtifact
type: entity
status: canonical
tags: [TradeForge, entity, decision-lifecycle, m10a, cognitive-artifact]
created: 2026-05-14
updated: 2026-05-14
aliases: [Cognitive Artifact, Decision Reasoning Artifact]
milestone: M10A
kb-issue: KB-M10A-001
---

# DecisionArtifact

## Definition

A DecisionArtifact is any structured operator reasoning record attached to a lifecycle stage.

DecisionArtifacts make discretionary cognition durable and replayable alongside lifecycle events.

## Artifact Taxonomy

| Artifact | Stage | Event |
|---|---|---|
| [[StructuredThesis]] | Thesis | decision.thesis_created |
| [[StructuredTradePlan]] | Plan | decision.plan_created |
| [[ScenarioBranch]] | Thesis or Plan | decision.scenario_branch_created |
| [[ReviewReflection]] | Review | review.reflection_created |

## Persistence Convention

All DecisionArtifacts are persisted as structured payload inside immutable lifecycle events.

This convention:
- preserves event sourcing purity
- makes artifacts replayable from event history
- avoids separate artifact storage infrastructure
- enables cognitive snapshot reconstruction at any historical timestamp

## Authority

- DecisionArtifacts are canonical only when embedded in immutable events
- Projected artifact views are derived read models
- Artifacts do not authorize execution or lifecycle transitions
- Artifacts do not mutate canonical state

## Related Concepts

- [[TradeThesis]]
- [[StructuredThesis]]
- [[StructuredTradePlan]]
- [[ReviewReflection]]
- [[CognitiveSnapshot]]
- [[Event Ledger]]
