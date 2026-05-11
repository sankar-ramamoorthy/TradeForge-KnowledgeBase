---
title: Implemented - TF-0022 Operational Attention Queues
type: processed-implementation
status: processed
created: 2026-05-11
source: knowledge/raw/20260511-implemented-tf-0022-operational-attention-queues.md
issue: TF-0022
milestone: M6
branch: feature/tf-0022-operational-attention-queues
related:
  - [[Persona Workspace]]
  - [[Projection]]
  - [[Human Decision Sovereignty]]
  - [[Decision Lifecycle State]]
---

# Implemented - TF-0022 Operational Attention Queues

## Stable Implementation Knowledge

TF-0022 established operational attention queues as runtime derived state. Queue items now explain why human attention is required and preserve source event references, lifecycle context, and workspace route targets.

## Architectural Meaning

Operational attention is a derived responsibility surface. It helps the Operating Workspace answer what matters now without becoming canonical truth, lifecycle authority, or execution permission.

## Persona And Replay Consequences

Persona context can shape deterministic queue priority using existing persona risk framing and decision velocity. This influence is interpretive only and replay-compatible because queue output remains deterministic for the same event history and persona context.

Attention queues can be rebuilt from Event Ledger history through workspace projections. They do not depend on live APIs, UI state, AI output, or hidden mutable storage.

## Boundary Preservation

The implementation preserves these boundaries:

- Event Ledger remains canonical truth.
- Workspace projections remain derived state.
- Attention queues remain derived state.
- Decision Lifecycle Engine remains lifecycle authority.
- Queue items do not authorize execution.
- AI prioritization remains out of scope.

## Validation

Runtime validation completed successfully:

- `uv run pytest` - 138 passed
- `uv run ruff check .` - passed
- `uv run mypy src tests` - passed

## Follow-Up

Context-aware workspace summaries remain separate and belong to TF-0023. Future richer risk/review semantics should still remain event-derived and deterministic unless a later ADR changes the model.
