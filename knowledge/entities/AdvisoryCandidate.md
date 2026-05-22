---
title: AdvisoryCandidate
type: entity
status: canonical
tags: [TradeForge, entity, advisory, candidate, lifecycle-boundary, M12]
created: 2026-05-22
updated: 2026-05-22
milestone: M12
runtime-issues: [TF-A011, TF-A012, TF-A013, TF-A014, TF-A015]
source_history:
  - knowledge/processed/20260522-m12-advisory-candidate-workflow-synthesis.md
---

# AdvisoryCandidate

## Definition

An AdvisoryCandidate is a reviewable advisory artifact that represents a
possible opportunity, risk, or attention item before commitment-space entry.

It may be durable, queryable, replay-visible, and visible in operator workflows
while remaining outside the Decision Lifecycle.

## Authority

An AdvisoryCandidate is non-canonical advisory content.

It does not:

- create a TradeIdea
- develop or revise a TradeThesis
- approve a TradePlan
- execute a trade
- authorize lifecycle transition
- become buy/sell recommendation authority

## Workflow Boundary

Candidate ingestion belongs to advisory space:

```text
Observation -> Candidate -> Review Queue
```

Operator promotion enters commitment space only through the existing lifecycle
workflow:

```text
New Trade Idea -> TradeIdea
```

The bridge requires explicit operator action, editable prefilled context, and
traceability to the source candidate.

## Queue Semantics

Candidate review queues are derived advisory read models unless a later ADR
introduces persisted operator queue actions.

They must remain deterministic and must not hide AI ranking, imply lifecycle
priority, or substitute for human approval.

## Related Concepts

- [[AdvisoryArtifact]]
- [[AdvisoryObservation]]
- [[CognitiveEvidence]]
- [[TradeIdea]]
- [[Decision Lifecycle]]
- [[Human Decision Sovereignty]]
