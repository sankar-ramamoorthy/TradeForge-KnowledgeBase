---
title: M14C Research Cockpit Import Bridge Planning Synthesis
type: planning-synthesis
status: processed-draft
created: 2026-05-27
updated: 2026-05-27
tags:
  - TradeForge
  - M14C
  - Research-Cockpit
  - Import-Bridge
  - planning
  - advisory-artifact
  - Human-Decision-Sovereignty
  - replayability
sources:
  - "knowledge/raw/M14C — Research Cockpit Import Bridge.md"
  - "knowledge/raw/M14C - research-import-bridge-design.md"
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
  - [[Advisory Candidate]]
  - [[Decision Lifecycle Engine]]
  - [[Event Ledger]]
  - [[Replay System]]
  - [[Persona Workspace]]
---

# M14C Research Cockpit Import Bridge Planning Synthesis

## Summary

The three M14C notes converge on a controlled Research Cockpit Import Bridge that lets external research artifacts enter TradeForge as advisory, non-canonical draft material.

The stable intent is not to make TradeForge a long-form writing environment or autonomous research agent. The intent is to let operators use external cognition tools while preserving Human Decision Sovereignty, lifecycle integrity, replayability, provenance, and Event Ledger authority.

M14C should remain exploratory until the import schema, UX pattern, provenance model, and first vertical slice are validated.

## Runtime Alignment

The runtime repository already contains completed M12 foundations directly relevant to this planning pass:

- `TF-A016 - Define external research cockpit import boundary`
- `TF-A017 - Implement research artifact ingestion API`
- `TF-A018 - Implement Codex/Claude-generated advisory artifact support`
- `TF-A019 - Implement advisory markdown artifact persistence`
- `TF-A020 - Implement replay-safe advisory snapshot capture`

The M12 acceptance meaning was that machine-originated observations become first-class advisory artifacts, Human Decision Sovereignty remains preserved, evidence becomes replayable and inspectable, and advisory cognition becomes operationally usable without becoming authoritative.

M14C should therefore focus on the remaining bridge from advisory artifact infrastructure into operator-controlled lifecycle authorship and cross-workspace UX.

## Source Note Comparison

| Source | Primary Emphasis | Stabilizing Contribution |
| --- | --- | --- |
| `M14C — Research Cockpit Import Bridge.md` | Core bridge concept and `TF-R001` acceptance criteria | Defines imported research as draft material, not thesis |
| `M14C - research-import-bridge-design.md` | Cross-workspace UX doctrine and design-spec-first sequencing | Frames import as workspace interaction doctrine, not file utility |
| `M14C - ideas for a formal M14C planning pass.md` | Milestone planning and implementation sequence | Narrows first slice and proposes schema/state model |

## Convergence

- Imported research is advisory/source material, not canonical lifecycle state.
- The operator must explicitly review, edit, reject, accept, or promote imported material.
- Promotion must emit normal canonical lifecycle event types through the Decision Lifecycle Engine.
- The Event Ledger remains canonical truth.
- Replay must preserve provenance for source research.
- `TF-R001` should be a narrow thesis draft import slice.
- Plan import, execution import, auto approval, auto sizing, and auto conviction scoring are out of scope for the first slice.
- A structured Markdown/YAML import schema should be defined before implementation.

## Contradictions And Tensions

No hard doctrinal contradictions appear across the notes. The tensions are design-level and should remain unresolved until tested.

### Import Surface Location

One note suggests an Import Review workspace. Another emphasizes thesis workspace import preview and cross-workspace consistency.

This produces a UX tension:

- Dedicated Import Review workspace gives stronger boundary clarity.
- Embedded lifecycle previews preserve workflow continuity.
- A hybrid pattern may be best, but that is not proven.

### Filesystem-Only Versus Event-Backed Artifacts

The planning note says filesystem plus replay metadata may be enough for `TF-R001`. It also suspects TradeForge eventually wants event-backed advisory artifacts.

This produces an architecture tension:

- Filesystem-only is simpler if M14C is treated as local draft intake.
- Existing M12 advisory artifact persistence may already satisfy much of the event-backed/provenance need without inventing a new import subsystem.

### Import Events Naming

The design note names events such as `ResearchDraftImported`, `ImportedFieldAccepted`, and `ImportedDraftRejected`.

These are exploratory. They must be reconciled with current advisory event doctrine, which allows advisory capture facts but prevents advisory content or lifecycle intent from becoming canonical.

## Emerging Stable Doctrine

The M14C notes suggest the following stable doctrine candidates:

- External research may assist lifecycle cognition but must not own lifecycle intent, authorization, or canonicalization.
- Imported files are advisory artifacts or import artifacts until explicitly reviewed.
- Imported material may support authorship, but not approval.
- Import provenance is part of replayability.
- The UI must make imported, accepted, promoted, and canonical states visually and semantically distinct.
- Plan import is materially higher risk than thesis import and should be delayed.
- A Research Cockpit should remain outside lifecycle authority unless and until its outputs pass through explicit TradeForge review.

These are not yet canonical doctrine, but they are strong candidates for later design doctrine, ontology refinement, or ADR preparation.

## Stable Architectural Direction

M14C should proceed as a design-first, thin-slice milestone:

```text
external Markdown/YAML research artifact
    -> deterministic import parse
    -> non-canonical imported draft/advisory artifact
    -> operator preview and selective acceptance
    -> manual lifecycle creation/promotion
    -> canonical lifecycle event
    -> replay-visible provenance
```

## Architectural Considerations

### Import Artifact Boundary

The runtime model needs a non-canonical representation for imported material. The notes use `ImportedResearchDraft` as a placeholder, but that name is not yet canonical.

This representation must remain distinct from:

- `TradeIdea`
- `Thesis`
- `TradePlan`
- canonical lifecycle events
- execution authorization

### Schema Boundary

The first schema should be narrow and versioned. It should support thesis draft import without forcing universal import semantics.

Likely first-slice fields:

- `artifact_type`
- `schema_version`
- `symbol`
- `title`
- `source`
- `generated_at`
- `thesis.narrative`
- `catalysts`
- `risks`
- `assumptions`
- `invalidation`
- `evidence`
- `notes`

### Event Boundary

Canonical lifecycle events are only emitted after explicit operator promotion.

Possible import/advisory capture facts require separate design. They must not embed generated recommendations, lifecycle transition intent, execution authority, or buy/sell instructions into the Event Ledger.

## UX Ideas

The import UX should support:

- preview before acceptance
- field-level acceptance
- group-level acceptance
- full-section acceptance where appropriate
- visible provenance markers
- visible edited-after-import indicators
- reject path
- error path
- clear distinction between imported draft content and canonical lifecycle material

## Competing UX Options

### Option A: Dedicated Import Review Workspace

Use a separate workspace for all imported materials.

Best when boundary clarity matters more than direct workflow continuity.

### Option B: Embedded Lifecycle Preview

Show imports inside the target lifecycle workspace.

Best when the operator is already authoring a Thesis or Review and needs local context.

### Option C: Hybrid Import Review Pattern

Use a common import model and visual language, rendered within lifecycle surfaces when contextually appropriate.

This appears to be the strongest candidate, but the notes do not prove it yet.

## Canonical Boundary Concerns

The following behaviors should remain prohibited unless doctrine is explicitly changed:

- auto thesis creation
- auto plan creation
- auto approval
- auto conviction scoring
- auto sizing
- auto execution authorization
- importer writing canonical lifecycle events without operator promotion
- replay depending on live external research APIs
- external AI content becoming source-of-truth authority

## Risky Assumptions

- Operators will maintain review discipline when importing AI-assisted research.
- Markdown/YAML contracts are stable enough for early workflows.
- Replay provenance can be meaningful without full event-backed artifact storage in the first slice.
- A single import interaction pattern can scale across Thesis, Plan, Review, Replay, and Behavioral surfaces.
- The Research Cockpit can remain outside TradeForge without creating provenance gaps.

## Doctrinal Implications

- M14C strengthens the advisory artifact boundary.
- M14C extends replayability from decision events into cognition provenance.
- M14C creates a practical test for Human Decision Sovereignty in AI-assisted workflows.
- M14C may require future ontology work around imported artifacts, import states, and provenance overlays.
- M14C may justify an ADR after a thin implementation is tested.

## Open Questions

- What exact name should be used for the imported draft artifact?
- Should M14C reuse the completed M12 advisory artifact persistence directly, or add a separate thesis-draft import layer above it?
- Should import capture facts be Event Ledger events in `TF-R001`, or should M14C rely on advisory artifact persistence plus lifecycle-event traceability?
- What import state model is sufficient without creating a parallel lifecycle?
- How should imported field acceptance be recorded?
- Should rejected imports remain replay-visible?
- How should operator edits after import affect provenance?
- Where should M14C design docs live before canonical promotion?

## Future Extensions

- Plan workspace import support.
- Review reflection import support.
- Behavioral observation import support.
- Watchlist candidate ingestion.
- Advisory observation ingestion.
- Research Cockpit file contracts.
- Research Cockpit scripts outside TradeForge.
- MCP ingestion after the local import model stabilizes.
- ADR candidate for external research ingestion and canonical boundary governance.
