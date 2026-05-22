---
title: M13 Runtime Issue Spec Synthesis
type: processed-synthesis
status: processed
created: 2026-05-21
updated: 2026-05-22
source_history:
  - knowledge/raw/20260521-m13-runtime-issue-spec-planning.md
  - knowledge/processed/20260522-m13-preparation-readiness-synthesis.md
tags: [m13, runtime-planning, issue-discipline, contextual-interpretation, advisory-boundary]
related:
  - "[[Runtime KB Development Loop]]"
  - "[[AI Advisory Boundary]]"
  - "[[Contextual Interpretation]]"
---

# M13 Runtime Issue Spec Synthesis

## Stabilized Outcome

The M13 runtime issue register gap was closed at the planning level. `DOCS/ISSUE_REGISTER.md` now has individual detailed issue sections for TF-B001 through TF-B015 instead of one grouped placeholder.

This does not start M13 runtime implementation. M13 remains planned and must wait until M12 is implemented.

## Architectural Boundary

M13 models the chain:

```text
Observation -> Interpretation -> Contextual Weighting -> Thesis Influence
```

It does not model:

```text
Indicator -> Buy/Sell
```

The issue specs preserve the ADR-0042 rule that `AdvisoryInterpretation` is a non-canonical advisory artifact. The event ledger may record only the capture fact through `advisory.interpretation_captured`.

Interpretation content, rationale, caveats, provenance detail, summaries, and narratives remain outside `event_ledger`.

## Runtime Planning Implications

The M13 issue sequence is now implementable issue-by-issue after M12:

- TF-B001 establishes the interpretation artifact schema.
- TF-B002 through TF-B005 establish qualitative weight, regime context, conflict, and confidence metadata.
- TF-B006 through TF-B009 model thesis influence, evidence classification, drift, and contradiction without lifecycle authority.
- TF-B010 through TF-B015 add replay overlays, workspace surfaces, uncertainty-preserving UX, summaries, narratives, and reasoning timelines.

## Invariant Preservation

The specs explicitly preserve:

- Human Decision Sovereignty.
- Event Ledger Canonical Truth.
- Replayability Is Foundational.
- AI Advisory Boundary.
- Derived State Must Remain Distinguishable.
- Lifecycle Authority.
- Historical Integrity.

## ADR Assessment

No new ADR is required for this planning pass. ADR-0042 already covers contextual interpretation and thesis influence. ADR-0041 remains the dependency boundary for M12 advisory observations and evidence.

## Open Sequencing Constraint

M13 interpretation implementation should not begin until:

- M12 runtime implementation is complete.
- M12 advisory observation and evidence persistence exists.
- `advisory.observation_captured` and replay-visible advisory observation timelines are available.

## M13 Readiness Update - 2026-05-22

The runtime issue register now includes TF-F045 through TF-F054 as M13 LLM
adapter prerequisite issues. The roadmap now groups M13 into LLM adapter
prerequisites, interpretation domain/infrastructure, and replay/UX surfaces.

The correct first M13 issue is TF-F045: add the LiteLLM credential shape to the
CredentialStore. TF-F046, the concrete OpenAI-compatible advisory provider,
blocks TF-B014 evidence narrative generation.
