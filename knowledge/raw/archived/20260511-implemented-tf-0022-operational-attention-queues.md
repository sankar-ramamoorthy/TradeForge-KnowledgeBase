---
title: TF-0022 Operational Attention Queues Implementation Capture
type: raw-implementation-capture
status: raw
created: 2026-05-11
issue: TF-0022
milestone: M6
branch: feature/tf-0022-operational-attention-queues
source_runtime_repo: C:/Users/bosto/dockerstuff/TradeForge
---

# TF-0022 Operational Attention Queues Implementation Capture

## Implementation Summary

Implemented operational attention queues as deterministic derived state over workspace projections and event history.

Runtime changes:

- Added `src/services/workspace_engine/attention.py`.
- Exported attention queue models and services through `src/services/workspace_engine/__init__.py`.
- Added `tests/test_operational_attention_queues.py`.

## Architecture Result

Operational attention is represented by immutable queue items with category, reason, priority, route target, explanation, lifecycle context, and source event references. Queue output is explicitly derived and includes authority boundaries stating that queue items do not authorize execution or lifecycle transitions.

The queue projector consumes `WorkspaceProjectionSet` from TF-0021. The read service composes the EventStore port with workspace projection generation and does not append events.

## Event And Lifecycle Impact

No new event types were introduced. No lifecycle transition semantics changed.

Queue items are produced from existing decision, scenario, market, execution, and review event facts. Lifecycle stage is derived for context only and remains non-authoritative.

## Persona Impact

Persona context shapes deterministic priority ordering through existing persona risk framing and decision velocity fields. Persona influence remains interpretive only and does not mutate facts.

## Replay Impact

Attention queues are deterministic for the same ordered event history and persona context. They are rebuild-compatible through the existing projection pipeline pattern and do not depend on live APIs, UI state, or AI output.

## Validation Executed

- `uv run pytest tests\\test_operational_attention_queues.py` - passed
- `uv run pytest` - 138 passed
- `uv run ruff check .` - passed
- `uv run mypy src tests` - passed

## Follow-Up Boundaries

TF-0023 should implement context-aware workspace summaries separately. TF-0022 intentionally does not add AI prioritization, UI, API endpoints, persistence, broker execution, or new event taxonomy.
