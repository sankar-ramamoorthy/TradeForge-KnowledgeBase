---
title: CognitiveEvidence
type: entity
status: canonical
tags: [TradeForge, entity, advisory, evidence, cognition, M12]
created: 2026-05-19
updated: 2026-05-19
milestone: M12
runtime-issues: [TF-A001, TF-A003, TF-A004]
---

# CognitiveEvidence

## Definition

CognitiveEvidence is a source-linked advisory reference attached to an
AdvisoryObservation.

It preserves why the observation existed without turning the observation into a
canonical decision fact.

## Composition

CognitiveEvidence should preserve:

- evidence identity
- source kind
- source identifier
- short source summary
- optional observed timestamp

The evidence may point to market context, fundamentals context, review
artifacts, replay timeline entries, operator prompts, provider outputs, or other
bounded advisory sources.

## Authority Boundary

CognitiveEvidence is not proof that a trade should be taken.

It may support later interpretation, but M12 does not classify evidence as
supporting, weakening, conflicting, stale, or decisive. Those meanings are
deferred to later interpretation milestones.

## Related Concepts

- [[AdvisoryObservation]]
- [[MarketContext]]
- [[ReviewArtifact]]
- [[ReplayTimeline]]
- [[Human Decision Sovereignty]]
