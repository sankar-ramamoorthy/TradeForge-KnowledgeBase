---
title: M14 Behavioral Intelligence Planning Synthesis
type: processed-synthesis
status: processed
created: 2026-05-26
source:
  - knowledge/raw/20260526-m14-behavioral-intelligence-planning.md
  - knowledge/raw/M14 Proposed plan.md
issue: TF-C001
milestone: M14
tags:
  - TradeForge
  - M14
  - behavioral-intelligence
  - cognitive-auditability
  - behavioral-signals
  - derived-read-models
---

# M14 Behavioral Intelligence Planning Synthesis

## Summary

M14 begins the Behavioral Intelligence And Cognitive Auditability milestone by
making recurring process problems visible during review without creating new
lifecycle authority, execution authority, AI authority, or hidden canonical
state.

The first implemented slice is TF-C001: Detect recurring sizing violations.
This slice deliberately starts with deterministic, replayable behavioral signals
derived from existing Event Ledger history rather than clustering, scoring, or
AI interpretation.

## Stabilized Boundary

TF-C001 and ADR-0044 stabilize a new derived-read-model boundary:

```text
Event Ledger history
  -> deterministic behavioral detector
  -> BehavioralSignal derived read model
  -> read-only review/replay/workspace API context
```

Behavioral signals are not events. They are rebuilt from canonical lifecycle,
plan, execution, and review facts. They preserve source event references,
severity, recurrence count, and explicit non-canonical authority metadata.

## Runtime Synchronization

The runtime repository now records:

- TF-C001 as Done in `DOCS/ISSUE_REGISTER.md`
- TF-C002 through TF-C010 as Planned M14 follow-on work
- ADR-0044 as the accepted decision for behavioral signals as derived read
  models
- `GET /behavioral/signals` as the read-only derived signal API
- `SizingViolationDetector` and `BehavioralSignalReadService` as the first
  runtime implementation of this boundary

The runtime roadmap should describe M14 as active/in progress while TF-C002
through TF-C010 remain planned.

## Ontology Implications

`BehavioralSignal` is ready for canonical entity promotion because the runtime
has an accepted ADR, a concrete implementation, tests, and stable authority
boundaries.

The following terms remain emerging and should not yet be promoted to canonical
entity definitions:

- `DisciplineDrift`
- `CognitiveAuditability`
- `ProcessDegradation`
- `DecisionQuality`
- `BehavioralReplay`

They should continue to stabilize through later M14 issues.

## Workflow Implications

M14 should continue in this order:

1. TF-C002: Detect impulsive execution patterns
2. TF-C003: Implement process deviation overlays
3. TF-C004 through TF-C010: expand into clustering, recurring mistake analysis,
   discipline deterioration, thesis attachment analysis, emotional reflection,
   behavior timelines, and decision-quality review metrics

The next implementation should preserve the TF-C001 pattern: deterministic
signals first, source-linked overlays second, higher-order interpretation only
after the deterministic foundation exists.

## Invariants Preserved

The M14 boundary preserves:

- Event Ledger Canonical Truth: behavioral signals do not become canonical facts.
- Replayability Is Foundational: signals rebuild from historical events.
- Derived State Must Remain Distinguishable: signal responses carry derived and
  non-canonical authority.
- Deterministic Rule Evaluation: TF-C001 is rule-based, auditable, and tested.
- Human Decision Sovereignty: no behavioral signal gates approval or execution.
- Reflection And Review Are First-Class: behavioral output supports review,
  learning, and process auditability.

## Promotion Result

Promoted stable knowledge to:

- `knowledge/entities/BehavioralSignal.md`
- `knowledge/topics/machine-assisted-discretionary-cognition.md`

Archived processed raw captures:

- `knowledge/raw/archived/20260526-m14-behavioral-intelligence-planning.md`
- `knowledge/raw/archived/M14 Proposed plan.md`

