---
title: TF-0065 Planning Synthesis - AI Advisory Interfaces
type: processed-synthesis
status: processed
tags: [TradeForge, M11, TF-0065, AI-advisory, architecture]
created: 2026-05-19
updated: 2026-05-19
source_history:
  - knowledge/raw/20260519-tf-0065-ai-advisory-interfaces-planning.md
related:
  - "[[AI Advisory Boundary]]"
  - "[[Human Decision Sovereignty]]"
  - "[[Replayability Is Foundational]]"
issues:
  - TF-0065
---

# TF-0065 Planning Synthesis - AI Advisory Interfaces

TF-0065 should establish the runtime AI advisory contract as a pure boundary before introducing any concrete model provider or user-facing AI feature.

The first M11 runtime shape should be intentionally small:

- provider-agnostic advisory request and response contracts
- explicit non-canonical authority classification
- provenance and uncertainty fields
- source references to event IDs, projection IDs, or advisory context inputs
- an orchestration service with no event-store or lifecycle mutation dependency

This preserves ADR-0006 and gives later M11 issues a stable contract for replay summarization, review assistance, and advisory provenance tracking.

No new event taxonomy, lifecycle transition, persistence adapter, or LLM provider adapter belongs in TF-0065.

