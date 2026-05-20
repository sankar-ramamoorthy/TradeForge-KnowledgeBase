---
title: Advisory Candidate Promotion Boundary
type: topic
status: canonical
tags: [TradeForge, advisory, candidate, lifecycle-boundary, M12]
created: 2026-05-19
updated: 2026-05-19
milestone: M12
runtime-issues: [TF-A001, TF-A002, TF-A005]
---

# Advisory Candidate Promotion Boundary

## Principle

Advisory cognition and lifecycle commitment are separate spaces.

Advisory space may contain observations, context, evidence, interpretations, and
candidate attention artifacts.

Commitment space begins only when an operator creates a TradeIdea through the
decision lifecycle.

## Boundary Rule

No advisory observation, scanner output, candidate, AI response, provider
payload, or evidence bundle may automatically promote itself into a TradeIdea.

Promotion requires an explicit operator-controlled lifecycle action.

## M12 Scope

M12 may capture advisory observations and make capture facts replay-visible.

M12 does not implement:

- candidate queues
- autonomous promotion
- buy/sell recommendations
- thesis influence scoring
- support/weakening/conflict classification
- execution intent

## Replay Meaning

Replay may show that advisory context existed before a lifecycle decision.

Replay must not imply that the advisory artifact caused, authorized, or
validated the lifecycle transition.

## Related Concepts

- [[AdvisoryObservation]]
- [[CognitiveEvidence]]
- [[TradeIdea]]
- [[Decision Lifecycle]]
- [[AI Advisory Boundary]]
