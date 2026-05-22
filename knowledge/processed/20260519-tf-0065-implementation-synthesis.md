---
title: TF-0065 Implementation Synthesis - AI Advisory Interfaces
type: processed-synthesis
status: processed
tags: [TradeForge, M11, TF-0065, AI-advisory, implementation]
created: 2026-05-19
updated: 2026-05-19
source_history:
  - knowledge/raw/20260519-tf-0065-ai-advisory-interfaces-planning.md
  - knowledge/raw/20260519-tf-0065-ai-advisory-interfaces-implementation.md
related:
  - "[[AI Advisory Boundary]]"
  - "[[Human Decision Sovereignty]]"
  - "[[Replayability Is Foundational]]"
issues:
  - TF-0065
---

# TF-0065 Implementation Synthesis - AI Advisory Interfaces

TF-0065 established the first M11 runtime AI advisory boundary without adding a concrete LLM adapter or advisory persistence.

The implemented contract keeps AI output explicitly non-canonical through:

- immutable request and response models
- `AdvisoryAuthority.ADVISORY`
- required provenance fields
- required uncertainty caveats
- required source references
- a provider protocol that returns advisory artifacts rather than workflow actions
- a service orchestrator with no event store or lifecycle authority dependency

Later M11 issues can now implement replay summarization, review assistance, and advisory provenance tracking against this stable contract.

The boundary remains aligned with ADR-0006: AI may assist interpretation, but cannot mutate canonical state, approve lifecycle transitions, or execute trades.

