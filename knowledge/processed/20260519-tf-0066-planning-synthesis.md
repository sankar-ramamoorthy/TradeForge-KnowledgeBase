---
title: TF-0066 Planning Synthesis - Replay Summarization Assistance
type: processed-synthesis
status: processed
tags: [TradeForge, M11, TF-0066, replay, AI-advisory]
created: 2026-05-19
updated: 2026-05-19
source_history:
  - knowledge/raw/20260519-tf-0066-replay-summarization-planning.md
issues:
  - TF-0066
---

# TF-0066 Planning Synthesis - Replay Summarization Assistance

Replay summarization should be built as a source-linked advisory layer over existing replay outputs. It must not change replay truth, add events, or persist summaries in TF-0066.

The correct implementation is a small services-layer adapter from `ReplayTimeline` into the TF-0065 advisory contract.

