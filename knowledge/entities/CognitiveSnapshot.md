---
title: CognitiveSnapshot
type: entity
status: canonical
tags: [TradeForge, entity, replay, m10a, cognitive-artifact]
created: 2026-05-14
updated: 2026-05-14
aliases: [Cognitive Reconstruction, Point-In-Time Cognition]
milestone: M10A
kb-issue: KB-M10A-005
---

# CognitiveSnapshot

## Definition

A CognitiveSnapshot is the reconstructed operator cognition state at a specific historical timestamp.

It answers: "what was the operator thinking at time T?"

## Composition (Planned — M10AIS10)

A CognitiveSnapshot at time T contains:
- the current lifecycle stage at T
- the most recent StructuredThesis payload before T
- the most recent StructuredTradePlan payload before T (if Plan stage reached)
- active ScenarioBranches before T
- market regime context at T (from market events before T)

## Reconstruction Principle

CognitiveSnapshot reconstruction is:
- deterministic: derived only from immutable events before T
- replayable: requires no live APIs or external state
- explicit: all content derives from event payloads
- non-authoritative: CognitiveSnapshot is a derived read model, not canonical truth

## Use In Replay

The Replay workspace uses CognitiveSnapshot reconstruction to answer:
- what thesis was active when the plan was created?
- what assumptions was the operator operating under at execution?
- what market context was visible at position entry?
- what scenario did the operator consider most likely?

This enables the review workspace to compare original reasoning against actual outcome.

## Authority Boundary

CognitiveSnapshot is a derived projection artifact.
It is NOT canonical truth.
It may not modify events or lifecycle state.

## Related Concepts

- [[StructuredThesis]]
- [[StructuredTradePlan]]
- [[ScenarioBranch]]
- [[ReviewReflection]]
- [[ReplaySession]]
- [[HistoricalReconstruction]]
