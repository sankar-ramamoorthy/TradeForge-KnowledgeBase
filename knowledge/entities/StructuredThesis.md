---
title: StructuredThesis
type: entity
status: canonical
tags: [TradeForge, entity, decision-lifecycle, m10a, cognitive-artifact]
created: 2026-05-14
updated: 2026-05-14
aliases: [Thesis Artifact, Trade Thesis Content]
milestone: M10A
kb-issue: KB-M10A-001
---

# StructuredThesis

## Definition

A StructuredThesis is the durable cognitive artifact that captures operator reasoning during the
Idea→Thesis lifecycle transition.

It extends [[TradeThesis]] with explicit semantic fields that make operator reasoning replayable.

## Fields

| Field | Type | Required | Description |
|---|---|---|---|
| narrative | str | yes | the core thesis statement — why this idea has merit |
| catalysts | list[str] | yes | identified thesis drivers (catalyst events, conditions) |
| assumptions | list[str] | yes | assumptions that must hold for the thesis to remain valid |
| invalidation_conditions | list[str] | yes | conditions that would invalidate the thesis |
| confidence_level | int (1–5) | yes | operator conviction (1=speculative, 5=high conviction) |
| regime_alignment | str | no | market regime context at time of thesis formation |

## Persistence

StructuredThesis content is persisted as structured payload inside the
`decision.thesis_created` lifecycle event.

The event ledger is the canonical source of truth.

No separate thesis artifact storage table exists in M10A phase.

## Authority Boundary

StructuredThesis is NOT executable.

It does not authorize:
- positions
- orders
- broker interaction
- plan creation (a separate lifecycle stage)

It is a recorded cognitive artifact — a structured statement of why the operator found the idea worth developing.

## Lifecycle Relationship

StructuredThesis is attached to the Thesis lifecycle stage.

```text
TradeIdea (Idea stage)
    ↓ develop-thesis authoring workflow
StructuredThesis (Thesis stage → decision.thesis_created event)
    ↓ create-plan authoring workflow (M10AIS06)
TradePlan (Plan stage)
```

## Replay Significance

The StructuredThesis content in the event payload allows replay to reconstruct:
- what the operator's thesis was before planning
- what assumptions were active at plan creation time
- what conditions the operator believed would invalidate the position
- how conviction level evolved (when thesis revisions are captured)

This enables review workspaces to compare original thesis against outcome.

## Related Concepts

- [[TradeThesis]]
- [[TradeIdea]]
- [[TradePlan]]
- [[ReviewReflection]]
- [[CognitiveSnapshot]]
- [[DecisionArtifact]]
