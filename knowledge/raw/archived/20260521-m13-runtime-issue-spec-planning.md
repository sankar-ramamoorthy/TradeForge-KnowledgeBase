---
title: M13 Runtime Issue Spec Planning Capture
type: raw-planning-capture
status: archived
created: 2026-05-21
tags: [m13, runtime-planning, issue-discipline, contextual-interpretation]
---

# M13 Runtime Issue Spec Planning Capture

## Context

Point 3 from the 2026-05-21 restart sequence required expanding runtime issue specs for M13:

- TF-B001 through TF-B015 existed in `DOCS/ISSUE_REGISTER.md` index rows.
- The detailed issue section was still grouped as `TF-B001 - TF-B015`.
- Runtime implementation has not started and must wait until M12 is complete.

The task used the canonical runtime-KB development loop at:

`playbooks/development/runtime-kb-development-loop.md`

## Source Authorities

- `DOCS/Milestone_Roadmap_v2.md` M13 section.
- `DOCS/ISSUE_REGISTER.md`.
- `DOCS/adr/0042-contextual-interpretation-and-thesis-influence.md`.
- `DOCS/adr/0041-advisory-observation-and-cognitive-evidence-foundation.md` as the M12 dependency and advisory observation boundary.

## Design Boundaries

M13 issue specs must preserve:

- advisory-only interpretation
- non-canonical interpretation artifacts
- canonical capture facts only through `advisory.interpretation_captured`
- operator acceptance/editing before AI-generated narrative persistence
- replay visibility without live providers or current AI output
- qualitative context, weight, influence, and uncertainty

M13 issue specs must exclude:

- deterministic predictive scoring
- hidden numeric ranking
- autonomous trade recommendations
- lifecycle transition authority
- thesis revision authority
- plan approval authority
- execution authority

## Runtime Change

`DOCS/ISSUE_REGISTER.md` was updated by replacing the grouped M13 placeholder with individual detailed issue sections for:

- TF-B001: Define interpretation artifact schema
- TF-B002: Implement contextual weighting framework
- TF-B003: Implement regime-aware weighting model
- TF-B004: Implement conflicting evidence analysis
- TF-B005: Implement confidence-range representation
- TF-B006: Implement thesis evidence influence tracking
- TF-B007: Implement supporting vs weakening evidence classification
- TF-B008: Implement thesis drift detection
- TF-B009: Implement contextual contradiction surfacing
- TF-B010: Implement evidence impact replay overlays
- TF-B011: Implement interpretation-first operational surfaces
- TF-B012: Implement uncertainty-preserving UX patterns
- TF-B013: Implement probabilistic cognition summaries
- TF-B014: Implement evidence narrative generation
- TF-B015: Implement contextual reasoning timelines

No runtime code, tests, roadmap status, ADRs, or lifecycle semantics were changed.

