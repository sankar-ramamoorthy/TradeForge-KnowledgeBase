---
title: TF-F015 Synthesis - Operational Attention Mypy Return Path
type: processed-synthesis
status: processed
tags: [TradeForge, TF-F, feedback, bug, operational-attention, mypy]
created: 2026-05-19
updated: 2026-05-19
source_history:
  - knowledge/raw/20260516-tf-f006-implementation.md
  - knowledge/raw/20260519-tf-f015-diagnosis.md
  - knowledge/raw/20260519-tf-f015-planning.md
related:
  - "[[Deterministic Rule Evaluation]]"
  - "[[Derived State Must Remain Distinguishable]]"
  - "[[Architectural Simplicity]]"
issues:
  - TF-F015
---

# TF-F015 Synthesis - Operational Attention Mypy Return Path

## Feedback Identified

TF-F006 focused verification exposed an unrelated strict mypy failure in `src/services/workspace_engine/attention.py`: `_decision_item_spec()` had no explicit fallback return path despite declaring a nullable tuple return type.

## Root Cause

The operational attention decision spec handled all current lifecycle stages, including `LifecycleStage.REVIEW`, but the `match` statement lacked a final fallback branch. Mypy therefore could not prove every path returned `tuple[...] | None`.

## Resolution

The runtime now returns `None` from an explicit wildcard fallback in `_decision_item_spec()`. Known lifecycle-stage behavior remains unchanged:

- Idea, Thesis, Plan, Approval, Execution, and Position still produce their existing decision attention specs.
- Review still produces no decision attention item.
- Unsupported or future unmatched stages fail closed by producing no derived decision attention item.

## Invariants

The fix preserves deterministic rule evaluation by keeping the attention mapping explicit. It preserves derived-state boundaries because no canonical events, lifecycle transitions, or persistence behavior changed.

## Replay And Lifecycle Implications

No replay or lifecycle behavior changed. Operational attention remains a deterministic projection over event-derived workspace state.

## Closure

The original `uv run mypy src\services\workspace_engine\attention.py` observation is resolved, and focused operational attention queue tests confirm existing queue behavior remains stable.

