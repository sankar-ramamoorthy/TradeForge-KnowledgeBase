---
title: M14C Formal Planning Pass Ideas
type: brainstorming-note
status: processed-draft
created: 2026-05-27
updated: 2026-05-27
tags:
  - TradeForge
  - M14C
  - planning
  - Import-Bridge
  - implementation-roadmap
  - replayability
source:
  - "knowledge/raw/M14C - ideas for a formal M14C planning pass.md"
related_milestones:
  - M14C
related_issues:
  - TF-A016
  - TF-A017
  - TF-A018
  - TF-A019
  - TF-A020
  - TF-R001
  - TF-R002
  - TF-R003
  - TF-R004
  - TF-R005
related:
  - [[Advisory Artifact]]
  - [[Decision Lifecycle Engine]]
  - [[Replay System]]
---

# M14C Formal Planning Pass Ideas

## Summary

This note recommends a formal M14C planning pass before implementation.

M14C is positioned between M14 Behavioral Intelligence and M15 future cognition/workflow expansion. Its purpose is to introduce controlled ingestion of external research artifacts into TradeForge while preserving lifecycle discipline, canonical boundaries, replayability, and explicit operator approval.

## Runtime Alignment

M12 already completed several foundational advisory artifact and Research Cockpit ingestion issues:

- `TF-A016 - Define external research cockpit import boundary`
- `TF-A017 - Implement research artifact ingestion API`
- `TF-A018 - Implement Codex/Claude-generated advisory artifact support`
- `TF-A019 - Implement advisory markdown artifact persistence`
- `TF-A020 - Implement replay-safe advisory snapshot capture`

The M14C planning pass should avoid duplicating this foundation. Its likely focus is the controlled thesis/lifecycle import experience, selective acceptance, field provenance, and promotion into normal lifecycle workflows.

## Key Concepts

- M14C should be defined as a milestone before code work begins.
- The first implementation should be a thin vertical slice, not universal import.
- `TF-R001 - Thesis Draft Import` is the recommended first slice.
- The import schema is identified as the most important technical design decision.
- Imported artifact state must make `Imported != Canonical` true everywhere.

## UX Ideas

The first slice should follow this path:

```text
filesystem markdown
    -> parsed research artifact
    -> thesis workspace import preview
    -> selective field acceptance
    -> manual thesis creation
    -> replay provenance
```

The note recommends designing:

- annotated wireframes
- interaction sequences
- import preview behavior
- provenance indicators
- field acceptance behavior

## Architectural Considerations

### First Thin Slice

`TF-R001` should include only deterministic filesystem Markdown import into a thesis draft preview flow.

Explicitly excluded from the first slice:

- watcher daemon
- background sync
- AI parsing
- plan import
- execution import

### Canonical Import Schema

A stable external artifact contract should be defined before UI implementation.

Example fields from the note:

```yaml
artifact_type: thesis_draft
schema_version: 1
symbol: ATKR
source:
  system: claude_code
  model: claude-sonnet-4
  generated_at: 2026-05-26T18:00:00Z
thesis:
  narrative: "..."
catalysts:
  - earnings acceleration
assumptions:
  - market remains risk-on
invalidation:
  - breaks below 200-day moving average
notes:
  - generated from external research cockpit
```

### Import State Model

The note proposes exploratory import states:

```text
Detected
Parsed
Previewed
Accepted
Rejected
Promoted
Archived
```

These states should not be confused with canonical lifecycle states.

## Canonical Boundary Concerns

The note explicitly prohibits:

- auto thesis creation
- auto plan creation
- auto approval
- auto conviction scoring
- auto sizing
- auto execution authorization

These prohibitions are doctrinally aligned with Human Decision Sovereignty, advisory boundaries, and lifecycle integrity.

## Stable Architectural Direction

- Do a formal M14C milestone definition before implementation.
- Build one deterministic, visible, operator-controlled, replayable slice first.
- Define schema before UI.
- Keep plan and execution import out of the first slice.
- Preserve imported artifact state separately from canonical lifecycle state.

## Unresolved Design Questions

- Should imported artifacts live filesystem-only in TF-R001?
- Should imported artifacts become event-backed advisory artifacts long term?
- What is the final import state vocabulary?
- Which TF-R issue sequence should be adopted in the runtime issue register?
- How much replay provenance is required in the first slice?

## Risky Assumptions

- Filesystem plus replay metadata is enough for TF-R001.
- Event-backed advisory artifacts can be deferred without weakening replayability.
- A schema can be stable enough for the first slice while remaining evolvable.
- Plan import can be safely delayed without blocking the Research Cockpit direction.

## Doctrinal Implications

- This note strongly supports incremental canonicalization and design-before-implementation discipline.
- It reinforces the distinction between advisory artifact state and canonical lifecycle event state.
- It extends replayability expectations to source research provenance.
- It treats import boundary mistakes as difficult to undo once AI-generated cognition enters lifecycle systems.

## Open Questions

- What minimum schema versioning rules are needed?
- Should `Accepted` and `Promoted` be separate states?
- Should rejected artifacts remain replay-visible?
- Should parse errors be preserved as operational diagnostics?

## Future Extensions

Exploratory issue sequence from the note:

- `TF-R001 - Thesis draft filesystem import`
- `TF-R002 - Thesis import preview UX`
- `TF-R003 - Imported provenance overlays`
- `TF-R004 - Replay visibility for imports`
- `TF-R005 - Plan workspace import support`
