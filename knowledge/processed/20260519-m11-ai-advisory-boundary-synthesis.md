---
title: M11 Synthesis - AI Advisory Boundary
type: processed-synthesis
status: processed
tags: [TradeForge, M11, AI-advisory, milestone-closeout]
created: 2026-05-19
updated: 2026-05-19
source_history:
  - knowledge/raw/20260519-m11-ai-advisory-boundary-closeout.md
issues: [TF-0065, TF-0066, TF-0067, TF-0068]
---

# M11 Synthesis - AI Advisory Boundary

M11 established the runtime AI advisory boundary as a set of non-authoritative contracts and services.

The milestone deliberately produced infrastructure for future AI features rather than an AI product surface:

- advisory request and response contracts
- provenance and uncertainty modeling
- replay summarization request construction
- review assistance request construction
- advisory provenance storage boundary

The main architectural result is that later AI features have a stable place to attach without inventing authority. AI outputs are reviewable, source-linked, and provenance-bearing, but they remain outside canonical event truth and lifecycle control.

