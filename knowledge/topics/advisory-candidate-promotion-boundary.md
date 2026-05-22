---
title: Advisory Candidate Promotion Boundary
type: topic
status: canonical
tags: [TradeForge, advisory, candidate, lifecycle-boundary, M12]
created: 2026-05-19
updated: 2026-05-22
milestone: M12
runtime-issues: [TF-A001, TF-A002, TF-A005, TF-A011, TF-A012, TF-A013, TF-A014, TF-A015]
source_history:
  - knowledge/processed/20260522-m12-advisory-candidate-workflow-synthesis.md
  - knowledge/processed/20260522-m12-advisory-artifact-boundary-synthesis.md
---

# Advisory Candidate Promotion Boundary

## Principle

Advisory cognition and lifecycle commitment are separate spaces.

Advisory space may contain observations, context, evidence, interpretations, and
candidate attention artifacts.

Commitment space begins only when an operator creates a TradeIdea through the
decision lifecycle.

An [[AdvisoryCandidate]] is therefore a reviewable advisory artifact, not a
lifecycle-adjacent decision object.

## Boundary Rule

No advisory observation, scanner output, candidate, AI response, provider
payload, or evidence bundle may automatically promote itself into a TradeIdea.

Promotion requires an explicit operator-controlled lifecycle action.

The only valid bridge from advisory candidate space into commitment space is:

```text
AdvisoryCandidate -> operator review -> New Trade Idea workflow -> TradeIdea
```

The bridge may prefill editable context and preserve traceability to the source
candidate, but it must not bypass operator intent or lifecycle validation.

## M12 Scope

M12 captures advisory observations, evidence, candidate artifacts, imported
research artifacts, markdown advisory artifacts, generated advisory artifacts,
and replay-safe advisory snapshots while keeping all advisory content
non-canonical.

M12 may expose candidate review queues as derived, deterministic advisory views.
Those queues are not canonical truth, lifecycle authority, execution permission,
or AI ranking authority.

M12 does not implement autonomous promotion, buy/sell recommendations, thesis
influence scoring, support/weakening classification, or execution intent.

Conflict markers, caveats, and staleness metadata may be surfaced as advisory
context. Full contextual interpretation and thesis influence semantics remain
M13 concerns.

## Replay Meaning

Replay may show that advisory context existed before a lifecycle decision.

Replay must not imply that the advisory artifact caused, authorized, or
validated the lifecycle transition.

Replay-safe advisory snapshots should preserve enough metadata to inspect what
advisory context existed historically without depending on live providers,
current AI output, or mutable external sources.

## UX Meaning

Candidate provenance views should show source, evidence, artifact identifiers,
uncertainty, caveats, and captured timestamps.

They should avoid scores, buy/sell wording, execution affordances, and lifecycle
authority claims.

## Related Concepts

- [[AdvisoryObservation]]
- [[AdvisoryArtifact]]
- [[AdvisoryCandidate]]
- [[CognitiveEvidence]]
- [[TradeIdea]]
- [[Decision Lifecycle]]
- [[AI Advisory Boundary]]
