---
title: M14 Behavioral Intelligence Completion
status: raw
type: implementation-capture
created: 2026-05-26
tags:
  - TradeForge
  - M14
  - behavioral-intelligence
  - cognitive-auditability
  - runtime-capture
---

# M14 Behavioral Intelligence Completion

## Scope

Processed remaining M14 runtime issues TF-C002 through TF-C010 after TF-C001
established the initial sizing-violation behavioral signal foundation.

## Design Boundary

Behavioral intelligence remains a deterministic derived read-model layer over
Event Ledger history. No new canonical event types, lifecycle transitions,
approval gates, persistence tables, or execution authority were introduced.

## Runtime Results

- Added impulsive execution signals from lifecycle timing, plan context, and
  operator-authored review language.
- Added deterministic behavioral clustering by persona, workspace, and signal
  type.
- Added recurring mistake analysis from behavioral signals and structured
  review reflections.
- Added discipline deterioration signals using explicit recent/baseline window
  counts.
- Added thesis attachment analysis from thesis revisions, confidence movement,
  invalidation review coverage, and review reflections.
- Added emotional reflection overlays sourced only from operator-authored
  review text.
- Added operator behavior timeline projection from deterministic behavioral
  signals.
- Added decision-quality review metrics that separate decision quality,
  execution quality, and bounded outcome context.
- Added review and replay frontend surfaces with derived/non-canonical
  authority labeling.

## Validation

- `uv run pytest tests\test_behavioral_signals.py`
- `uv run mypy src\domain\behavioral src\services\behavioral src\app\api\routes.py tests\test_behavioral_signals.py`
- `uv run ruff check src\domain\behavioral src\services\behavioral tests\test_behavioral_signals.py`
- `npm.cmd run typecheck`
- `npm.cmd run build`

## Semantic Notes

The durable semantic decision remains ADR-0044: behavioral outputs are derived
review context, not canonical facts. Emotional language is treated only as
operator-authored review context; the system does not infer emotional state as
truth.
