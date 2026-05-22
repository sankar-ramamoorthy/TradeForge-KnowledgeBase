---
title: AdvisoryObservation
type: entity
status: canonical
tags: [TradeForge, entity, advisory, cognition, M12]
created: 2026-05-19
updated: 2026-05-22
milestone: M12
runtime-issues: [TF-A001, TF-A002, TF-A003, TF-A004, TF-A005, TF-A006, TF-A007, TF-A008, TF-A009, TF-A010]
source_history:
  - knowledge/processed/20260521-tf-a001-advisory-observation-closeout-synthesis.md
  - knowledge/processed/20260521-tf-a002-tf-a004-advisory-observation-closeout-synthesis.md
  - knowledge/processed/20260521-tf-a005-tf-a007-advisory-observation-closeout-synthesis.md
  - knowledge/processed/20260521-tf-a008-tf-a010-advisory-observation-closeout-synthesis.md
---

# AdvisoryObservation

## Definition

An AdvisoryObservation is a durable advisory cognition artifact captured before
or alongside the formal decision lifecycle.

It records that a machine system or human operator noticed something potentially
relevant to trading cognition, while preserving evidence, provenance,
uncertainty, and caveats.

## Authority

An AdvisoryObservation is non-canonical advisory content.

Only the capture fact may become canonical through
`advisory.observation_captured`.

An AdvisoryObservation does not:

- create a TradeIdea
- develop or revise a TradeThesis
- approve a plan
- execute a trade
- rank as a buy/sell recommendation
- become lifecycle authority

## Required Context

An AdvisoryObservation must preserve:

- observation identity
- artifact identity
- observation kind
- capture origin
- persona context
- workspace context
- evidence/source references
- provenance summary
- qualitative uncertainty band
- caveats
- captured timestamp

Capture origin must use a stable value such as `operator_manual`, `provider_import`, `codex_generated`, `claude_generated`, `imported_research`, `replay_annotation`, or `future_scanner`. It preserves how the artifact entered advisory space for later trust modeling, evidence-quality review, source weighting, behavioral analysis, and replay analysis.

Decision and thesis links are optional contextual references only.

Lightweight contextual artifacts, conflict markers, caveat-derived unresolved
conflict markers, and derived evidence staleness metadata may be attached to an
AdvisoryObservation as advisory metadata.

These fields do not create `AdvisoryInterpretation` semantics, thesis influence,
contextual weighting, confidence scoring, recommendation authority, lifecycle
authority, or execution authority.

## Replay Meaning

Replay may show that an AdvisoryObservation existed at a historical time and may
link to the advisory artifact.

Replay must distinguish:

- canonical capture fact
- non-canonical advisory content
- later operator lifecycle decisions

## Related Concepts

- [[CognitiveEvidence]]
- [[AdvisoryArtifact]]
- [[TradeIdea]]
- [[TradeThesis]]
- [[ReplaySession]]
- [[AI Advisory Boundary]]


