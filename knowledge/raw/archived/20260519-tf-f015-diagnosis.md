---
title: TF-F015 Diagnosis - Operational Attention Mypy Return Path
type: raw-diagnosis
status: raw
created: 2026-05-19
issue: TF-F015
source_history:
  - knowledge/raw/20260516-tf-f006-implementation.md
---

# TF-F015 Diagnosis - Operational Attention Mypy Return Path

## Observable Gap

Focused verification during TF-F006 found that `uv run mypy src\services\workspace_engine\attention.py` reported:

```text
src\services\workspace_engine\attention.py:291: error: Missing return statement  [return]
```

The runtime issue is a type-check failure, not an observed queue behavior failure.

## Root Cause

`_decision_item_spec()` declares `tuple[AttentionReason, WorkspaceRouteId, AttentionPriority, str] | None`, but its `match stage:` statement only returns for the known `LifecycleStage` cases. `LifecycleStage.REVIEW` returns `None`, while mypy still requires an explicit fallback path for exhaustiveness.

## Relevant Invariants

- Deterministic Rule Evaluation: attention routing must stay explicit and auditable.
- Architectural Simplicity: the fix should be a minimal type-contract correction, not an attention-queue refactor.
- Derived State Must Remain Distinguishable: operational attention remains derived queue state, not canonical truth.

## Minimum Correct Scope

Add an explicit fallback return path to `_decision_item_spec()` that returns `None`.

## Out Of Scope

- Changing lifecycle semantics.
- Changing event models.
- Changing queue ordering, priority, routing, or source-event behavior.
- Refactoring the operational attention projector.

