---
title: ScenarioBranch
type: entity
status: canonical
tags: [TradeForge, entity, decision-lifecycle, m10a, scenario]
created: 2026-05-14
updated: 2026-05-14
aliases: [Scenario Branch, Conditional Reasoning Path]
milestone: M10A
kb-issue: KB-M10A-003
---

# ScenarioBranch

## Definition

A ScenarioBranch captures conditional reasoning structures attached to a lifecycle stage.

It formalizes the "if X then Y" reasoning operators use when developing discretionary theses.

## Fields (Planned — M10AIS04)

| Field | Type | Description |
|---|---|---|
| branch_type | enum | primary / alternative / invalidation / regime_transition |
| condition | str | the condition triggering this branch |
| implication | str | what the operator would do under this condition |
| confidence | int (1–5) | how likely the operator expects this branch |
| linked_stage | LifecycleStage | which lifecycle stage this branch is relevant to |

## Semantic Role

ScenarioBranch prevents binary thesis thinking.

It forces operators to articulate:
- what they would do if the primary thesis holds
- what they would do if an alternative scenario unfolds
- what would definitively invalidate the thesis

## Authority Boundary

ScenarioBranch is advisory reasoning structure.

It does not:
- authorize execution
- create lifecycle transitions
- replace the primary thesis

## Replay Significance

ScenarioBranches allow replay to reconstruct what conditional reasoning was visible
before execution, enabling the review workspace to compare outcome against the
specific scenario that actually unfolded.

## Related Concepts

- [[StructuredThesis]]
- [[TradeThesis]]
- [[ReviewReflection]]
- [[CognitiveSnapshot]]
