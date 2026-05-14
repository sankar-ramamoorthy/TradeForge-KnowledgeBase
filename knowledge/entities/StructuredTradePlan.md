---
title: StructuredTradePlan
type: entity
status: canonical
tags: [TradeForge, entity, decision-lifecycle, m10a, cognitive-artifact]
created: 2026-05-14
updated: 2026-05-14
aliases: [Plan Artifact, Trade Plan Content]
milestone: M10A
kb-issue: KB-M10A-002
---

# StructuredTradePlan

## Definition

A StructuredTradePlan is the durable cognitive artifact that captures operator execution intent
during the Thesis→Plan lifecycle transition.

It extends [[TradePlan]] with explicit semantic fields that make plan reasoning replayable.

## Fields (Planned — M10AIS06)

| Field | Type | Required | Description |
|---|---|---|---|
| entry_rationale | str | yes | why this is the right entry point and price level |
| stop_rationale | str | yes | why this stop level represents thesis invalidation |
| target_rationale | str | yes | why this target represents thesis fulfillment |
| sizing_rationale | str | yes | how position size was determined relative to conviction |
| execution_assumptions | list[str] | yes | assumptions required for the plan to be executable |
| playbook_alignment | str | no | which operational playbook this plan aligns with |

## Persistence

StructuredTradePlan content is persisted as structured payload inside the
`decision.plan_created` lifecycle event.

The event ledger is the canonical source of truth.

## Authority Boundary

StructuredTradePlan is a decision artifact, NOT an execution instruction.

It does not:
- authorize broker execution
- bypass lifecycle approval
- replace the Approval stage

The Approval stage explicitly authorizes risk.
StructuredTradePlan records what risk the operator is considering taking.

## Lifecycle Relationship

```text
StructuredThesis (Thesis stage)
    ↓ create-plan authoring workflow (M10AIS06-07)
StructuredTradePlan (Plan stage → decision.plan_created event)
    ↓ Plan Review workspace approval gate
Approval (plan_approved event)
```

## Replay Significance

StructuredTradePlan allows replay to reconstruct:
- the original execution intent before approval
- whether the plan was followed during execution
- whether entry/stop/target assumptions held post-execution
- what playbook the operator was following

## Related Concepts

- [[TradePlan]]
- [[StructuredThesis]]
- [[ReviewReflection]]
- [[CognitiveSnapshot]]
- [[DecisionArtifact]]
