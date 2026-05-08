---
title: ReviewArtifact
type: entity
status: canonical
tags: [TradeForge, entity, review]
created: 2026-05-08
updated: 2026-05-08
aliases: [Review Artifact]
---

# ReviewArtifact

## Definition

A ReviewArtifact is a durable output of reflection on a decision, position, workflow, rule evaluation, or behavioral pattern.

## Semantic Role

ReviewArtifact makes learning a first-class system output rather than a secondary analytics report.

## Authority Boundary

ReviewArtifact may contain interpretation and reflection, but it does not rewrite historical events or become execution authority.

## Lifecycle Relationship

ReviewArtifact belongs to the Review stage and may refer backward to the full lifecycle history.

## Event Relationships

Related event examples:

- `ReviewStarted`
- `ReviewCompleted`
- `OutcomeEvaluated`
- `RuleViolationDetected`
- `BehavioralInsightRecorded`

## Replay And Review Relevance

ReviewArtifact should be reconstructable and traceable to the decision context, plan, execution facts, and outcome being reviewed.

## Related Concepts

- [[ReplaySession]]
- [[TradePlan]]
- [[PositionIntent]]
- [[ExecutionIntent]]

