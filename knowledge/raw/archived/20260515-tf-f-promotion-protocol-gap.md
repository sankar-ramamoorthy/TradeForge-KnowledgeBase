---
title: Governance Gap — TF-F Issue Promotion Protocol
type: raw-capture
status: raw
tags: [TradeForge, governance, development-loop, TF-F, promotion, playbook-gap]
created: 2026-05-15
---

# Governance Gap — TF-F Issue Promotion Protocol

## Observation

During M10B/M10C planning, a gap in the development playbooks was identified.

TF-F008 and TF-F009 were created as field-observed feedback issues (TF-F### series)
via the feedback-issue-development-loop. Through the planning process they were
elevated into a formal milestone (M10B) with:
- Roadmap entries
- Linked ADR (M10C/TF-F004 has ADR-0037)
- Full issue specifications
- Acceptance criteria

At implementation time, neither playbook covers this case cleanly:

- feedback-issue-development-loop: assumes reactive, diagnosis-first, lighter KB cycle
- runtime-kb-development-loop: assumes issues born as TF-#### planned from a milestone

## The Gap

Neither playbook defines what happens when a TF-F issue graduates into a formal milestone.

There is no documented:
- Promotion trigger (when does a TF-F become milestone work?)
- Handoff point (at what phase does the feedback loop hand to the runtime loop?)
- Residual obligation (does feedback closure still apply after promotion?)

## Interim Rule (to be formalized)

When a TF-F issue is:
- Assigned a milestone (M10B, M10C, etc.)
- Has a roadmap entry
- Has been spec'd with acceptance criteria

It graduates to the runtime-kb-development-loop for implementation, starting at
Phase 3 (planning before implementation). Phases 1-2 of the feedback loop (triage
and diagnosis) are considered complete — documented in raw notes.

Phase 9 of the feedback loop (feedback closure) still applies at the end —
verify the original observation is demonstrably resolved.

## Promotion Trigger Candidates

A TF-F issue graduates when ANY of these are true:
1. It is assigned to a named milestone in Milestone_Roadmap_v2.md
2. An ADR is created for it
3. Its scope requires more than one implementation session
4. It involves changes to multiple layers (domain + api + frontend)

## What Needs to Be Added to Playbooks

feedback-issue-development-loop.md:
- New section: "When a TF-F Issue Is Promoted to a Milestone"
- Define handoff point: after Phase 5 (KB planning capture), hand to runtime loop
- Note: feedback closure (Phase 9) still applies at implementation end

runtime-kb-development-loop.md:
- New note under Phase 1 (Select Work): TF-F issues promoted to milestone are valid
  work items for this loop; treat diagnosis raw notes as the planning capture already done

## Status

Raw capture. Not canonical.
Needs processing into playbook updates when time permits.
Not urgent — interim rule is clear enough to proceed.
