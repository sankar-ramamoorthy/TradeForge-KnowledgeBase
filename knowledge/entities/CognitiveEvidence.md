---
title: CognitiveEvidence
type: entity
status: canonical
tags: [TradeForge, entity, advisory, evidence, cognition, M12]
created: 2026-05-19
updated: 2026-05-22
milestone: M12
runtime-issues: [TF-A001, TF-A003, TF-A004, TF-A006, TF-A007, TF-A009, TF-A010]
source_history:
  - knowledge/processed/20260521-tf-a005-tf-a007-advisory-observation-closeout-synthesis.md
  - knowledge/processed/20260521-tf-a008-tf-a010-advisory-observation-closeout-synthesis.md
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

Evidence may also carry advisory conflict markers, caveat-derived unresolved
conflict markers, and derived staleness metadata when those values are surfaced
as context rather than lifecycle or recommendation authority.

## Authority Boundary

CognitiveEvidence is not proof that a trade should be taken.

It may support later interpretation, but M12 does not classify evidence as
supporting, weakening, decisive, or thesis-influencing. Those meanings are
deferred to later interpretation milestones.

M12 conflict and staleness visibility is advisory metadata only. It does not
create `AdvisoryInterpretation`, contextual weight, confidence range, or
recommendation semantics.

## Related Concepts

- [[AdvisoryObservation]]
- [[AdvisoryArtifact]]
- [[MarketContext]]
- [[ReviewArtifact]]
- [[ReplayTimeline]]
- [[Human Decision Sovereignty]]
