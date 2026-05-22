---
title: M13 Preparation Readiness Synthesis
type: processed-synthesis
status: processed
created: 2026-05-22
updated: 2026-05-22
source_history:
  - knowledge/raw/archived/preperation done for M13 2026-05-22.md
  - knowledge/processed/20260522-tf-f045-litellm-credential-shape-synthesis.md
tags: [TradeForge, M13, runtime-planning, issue-discipline, LLM, advisory-interpretation]
related:
  - "[[20260521-m13-runtime-issue-spec-synthesis]]"
  - "[[20260522-llm-adapter-strategy-and-capability-review-synthesis]]"
  - "[[AdvisoryInterpretation]]"
  - "[[Machine-Assisted Discretionary Cognition]]"
---

# M13 Preparation Readiness Synthesis

## Stabilized Outcome

M13 is prepared at the runtime planning level.

The runtime `ISSUE_REGISTER.md` now contains detailed M13 prerequisite issues
TF-F045 through TF-F054 in addition to the existing TF-B001 through TF-B015
interpretation work. `Milestone_Roadmap_v2.md` now groups M13 into:

- LLM adapter prerequisites
- interpretation domain and infrastructure
- replay and UX surfaces

This closes the planning gap identified by the LLM adapter strategy review.

## Starting Issue

The correct first M13 issue was:

```text
TF-F045: Add LiteLLM credential shape to CredentialStore
```

TF-F045 should precede the concrete `OpenAICompatibleAdvisoryProvider` because
the provider must retrieve `base_url`, `api_key`, and `default_model` through
the existing credential boundary rather than through code, environment files, or
ad hoc configuration.

## TF-F045 Completion Update - 2026-05-22

TF-F045 is now done in the runtime issue register.

M13 advisory adapter work should proceed to TF-F046:

```text
Implement OpenAICompatibleAdvisoryProvider
```

TF-F046 should consume the completed LiteLLM credential shape and preserve the
provider-agnostic advisory boundary.

## Dependency Clarification

TF-F046 implements the concrete OpenAI-compatible advisory provider and blocks
TF-B014.

TF-B014 depends on an operational provider because evidence narrative
generation exercises `InterpretationDraftService`, which already exists but
requires a concrete `AIAdvisoryProvider` for end-to-end validation.

## M12 Foundation Reuse

Several M13 issues are verification, service logic, or UX work rather than
ground-up infrastructure because M12 already built part of the interpretation
foundation:

- TF-B001: domain model, stores, capture service, and draft service already exist
- TF-B002: contextual weight enum and store filtering already exist
- TF-B005: advisory confidence range enum and storage already exist
- TF-B006: `ThesisInfluenceSummary` already exists
- TF-B007: thesis influence enum values and query filtering already exist

The M13 implementation pass should verify and extend these foundations instead
of duplicating them.

## Boundary Preservation

The preparation work does not change advisory authority.

M13 still models:

```text
Observation -> Interpretation -> Contextual Weighting -> Thesis Influence
```

It still does not model:

```text
Indicator -> Buy/Sell
```

LLM adapter work, prompt work, interpretation drafts, evidence narratives, and
workspace surfaces remain advisory-only. They must not create lifecycle state,
approve plans, execute trades, or write canonical advisory content into the
Event Ledger.

## Operational Synchronization

Runtime planning is now aligned across:

- `DOCS/ISSUE_REGISTER.md`
- `DOCS/Milestone_Roadmap_v2.md`
- `DOCS/llm-adapter-strategy.md`
- KB M13 issue synthesis
- KB LLM adapter strategy synthesis

No new ADR is required for this preparation pass. Existing advisory,
credential, and interpretation ADRs remain sufficient.
