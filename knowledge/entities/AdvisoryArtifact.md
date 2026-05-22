---
title: AdvisoryArtifact
type: entity
status: canonical
tags: [TradeForge, entity, advisory, cognition, replayability]
created: 2026-05-22
updated: 2026-05-22
milestone: M12
runtime-issues: [TF-A016, TF-A017, TF-A018, TF-A019, TF-A020]
source_history:
  - knowledge/processed/20260522-m12-advisory-artifact-boundary-synthesis.md
---

# AdvisoryArtifact

## Definition

An AdvisoryArtifact is durable non-canonical advisory content preserved for
operator review, provenance inspection, and replay-safe historical context.

It may contain imported research, generated advisory notes, markdown cognition,
advisory observations, advisory candidates, or replay-safe snapshots.

## Authority

An AdvisoryArtifact is not canonical truth.

It does not:

- create a TradeIdea
- revise a TradeThesis
- approve a TradePlan
- execute a trade
- write lifecycle events
- become recommendation authority

Only a bounded capture fact may enter the Event Ledger when a specific advisory
artifact type defines one, such as `advisory.observation_captured` or the M13
`advisory.interpretation_captured` event.

## Required Context

An AdvisoryArtifact should preserve:

- artifact identity
- artifact kind
- source or capture origin
- persona context
- workspace context
- provenance summary
- evidence or source references where available
- uncertainty and caveats where available
- captured timestamp
- replay-safe snapshot metadata when needed

## Replay Meaning

Replay may show that an AdvisoryArtifact existed in historical context and may
link to its stored content.

Replay must not require live providers, current AI output, mutable external
sources, or regenerated advisory content to reconstruct what was visible at the
time.

## Related Concepts

- [[AdvisoryObservation]]
- [[AdvisoryCandidate]]
- [[AdvisoryInterpretation]]
- [[CognitiveEvidence]]
- [[ReplaySession]]
- [[AI Advisory Boundary]]
