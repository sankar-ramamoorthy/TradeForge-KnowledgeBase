---
title: M14C Operator Cognition Bridge
type: milestone-planning-synthesis
status: authoritative-planning-draft
created: 2026-05-27
updated: 2026-05-27
tags:
  - TradeForge
  - M14C
  - operator-cognition-bridge
  - Research-Cockpit
  - Import-Bridge
  - UX-doctrine
  - lifecycle-integrity
  - advisory-artifact
  - Human-Decision-Sovereignty
  - replayability
sources:
  - "knowledge/processed/20260527-m14c-research-import-bridge-design-normalized.md"
  - "knowledge/processed/20260527-m14c-formal-planning-pass-normalized.md"
  - "knowledge/processed/20260527-m14c-research-import-bridge-planning-synthesis.md"
related_milestones:
  - M12
  - M14C
  - M15
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
  - [[Review Artifact]]
---

# M14C Operator Cognition Bridge

## Summary

M14C defines the operator-facing bridge between imported advisory cognition and TradeForge lifecycle workflows.

M14C is not greenfield ingestion infrastructure. M12 already established the advisory artifact substrate for imported research, generated advisory artifacts, markdown artifact persistence, and replay-safe advisory snapshots. M14C extends that substrate with lifecycle-aware UX and operator-controlled workflows for selective canonical promotion.

The core M14C problem is not "how does TradeForge ingest research?" The core problem is:

```text
How does imported research cognition assist operator authorship
without becoming canonical lifecycle truth until the operator explicitly accepts
and promotes it through normal lifecycle workflows?
```

M14C should become the planning and UX doctrine reference for:

- operator workflow orchestration
- lifecycle UX bridging
- selective canonical promotion
- provenance-aware cognition handling
- cross-workspace consistency for imported advisory material

## Relationship To M12 Foundations

M14C builds on completed M12 foundations:

- `TF-A016 - Define external Research Cockpit import boundary`
- `TF-A017 - Implement research artifact ingestion API`
- `TF-A018 - Implement Codex/Claude-generated advisory artifact support`
- `TF-A019 - Implement advisory markdown artifact persistence`
- `TF-A020 - Implement replay-safe advisory snapshot capture`

Those issues established that machine-originated and externally authored material may become first-class advisory artifacts while remaining non-canonical. They also established provenance, advisory artifact persistence, markdown persistence, and replay-safe advisory snapshot handling.

M14C must not duplicate that ingestion substrate. Instead, it should answer the next design question:

```text
How does an operator use an advisory artifact as structured input
while preserving lifecycle intent, authorship, and canonical authority?
```

M12 made advisory cognition operationally available. M14C makes that cognition lifecycle-usable without letting it become lifecycle-authoritative.

## Core Doctrine

Imported research artifacts are advisory cognition inputs. They are not canonical lifecycle truth.

M14C is governed by the following doctrine:

- Human Decision Sovereignty remains mandatory.
- The Decision Lifecycle Engine remains lifecycle authority.
- The Event Ledger remains canonical truth.
- Advisory artifacts may assist authorship but must not create lifecycle intent.
- Imports may prefill, suggest, structure, or contextualize material only as editable draft cognition.
- Canonical lifecycle events occur only after explicit operator action through normal lifecycle workflows.
- Replay must preserve provenance and distinguish advisory inputs from canonical lifecycle facts.
- UX must make imported, accepted, edited, promoted, rejected, and canonical states visually and semantically distinct.

Stable architectural direction:

```text
M12 advisory artifact substrate
    -> M14C operator preview and selective acceptance
    -> manual lifecycle workflow entry
    -> operator-owned canonical promotion
    -> replay-visible provenance
```

## Operator Workflow Model

The operator workflow should preserve the existing lifecycle sequence.

The operator still explicitly enters Thesis, Plan, Review, and related lifecycle workflows. Imported research does not replace lifecycle entry. It assists authorship inside those workflows.

Conceptual flow:

```text
External research / Research Cockpit / ChatGPT / Claude / Codex
    -> M12 advisory artifact substrate
    -> global import preview
    -> operator inspection
    -> selective field acceptance
    -> operator edits and confirmation
    -> normal lifecycle workflow
    -> canonical lifecycle event
    -> replay-visible provenance
```

The workflow must support:

- inspect
- accept field
- accept section
- edit
- reject
- defer
- promote through lifecycle workflow

The workflow must not support hidden lifecycle mutation.

## Cross-Workspace UX Pattern

M14C should use a hybrid interaction model.

The three source documents explored dedicated Import Review workspace, embedded lifecycle previews, and a hybrid pattern. The synthesis favors the hybrid model as the planning direction while preserving open design details.

Hybrid pattern:

- global import preview for boundary clarity
- lifecycle-local rendering where the imported material is relevant
- selective per-field acceptance
- provenance visibility at field and artifact level
- manual canonical promotion through the target lifecycle workflow

This model preserves a common mental model across workspaces without forcing all import handling into a separate workspace that could fragment cognition.

The UX pattern should remain consistent across:

- Thesis Workspace
- Plan Workspace
- Review Workspace
- Replay surfaces
- later behavioral/reflection surfaces

The UI must avoid making imported material look like canonical lifecycle state before promotion.

## Selective Canonical Promotion Model

Selective canonical promotion is the central M14C workflow.

Promotion means an operator explicitly chooses to use imported advisory cognition as part of a normal lifecycle action. The promoted lifecycle artifact is operator-owned. The imported artifact remains source material with provenance.

Selective promotion may occur at multiple levels:

- individual field
- grouped fields
- section
- full draft

Acceptance does not automatically mean canonicalization. A field may be accepted into a draft and still remain non-canonical until the operator completes the lifecycle workflow action.

Recommended distinction:

```text
Imported
    advisory cognition exists

Accepted
    operator selected material for draft use

Edited
    operator changed accepted material

Promoted
    operator completed a lifecycle workflow action

Canonical
    resulting lifecycle event is appended through normal authority path
```

This state vocabulary is planning guidance, not final event taxonomy.

## Canonical Boundary Constraints

M14C explicitly prohibits:

- automatic thesis creation
- automatic plan creation
- automatic approval
- automatic execution authorization
- automatic conviction assignment
- automatic sizing decisions
- importer-written canonical lifecycle events without operator promotion
- imported research becoming Event Ledger truth by itself
- replay depending on live external research APIs
- external AI content becoming source-of-truth authority

Imported Plan material is especially sensitive. Plan imports may contain entries, targets, invalidation ideas, risk notes, or execution-adjacent language. M14C must keep sizing, approval, authorization, and execution authority operator-owned.

Canonical lifecycle events must remain normal lifecycle events. M14C should not introduce a parallel lifecycle or a hidden promotion path.

## Provenance And Replay Semantics

Provenance is part of M14C's replayability requirement.

Replay should reconstruct:

- what advisory artifact existed
- where it came from
- when it was captured or imported
- which source system or model produced it when known
- what fields the operator accepted
- what fields the operator edited
- what lifecycle artifact was ultimately created
- which canonical event resulted from operator promotion

Replay must distinguish:

- advisory artifact content
- accepted draft material
- operator-authored edits
- canonical lifecycle event facts
- derived replay overlays

M14C should reuse M12 replay-safe advisory snapshot capture where possible. Additional lifecycle-import provenance should be added only where the M12 substrate cannot reconstruct the operator's selective promotion path.

Open design issue:

- Whether field-level acceptance requires durable event-backed capture in `TF-R001`, or whether draft metadata plus final lifecycle traceability is sufficient for the first slice.

## Thesis Workspace Import Flow

Thesis import is the recommended first lifecycle-facing use case.

The Thesis Workspace flow should:

- surface eligible imported thesis draft material from the M12 advisory artifact substrate
- show a global import preview or import panel
- allow field-level acceptance into the thesis authoring surface
- preserve provenance markers for accepted fields
- allow operator edits before lifecycle creation or revision
- require explicit operator submission through the normal Thesis workflow

Candidate imported fields:

- symbol
- title
- thesis narrative
- catalysts
- risks
- assumptions
- invalidation conditions
- evidence links
- notes

Canonical boundary:

- imported thesis draft material is not a `Thesis`
- accepted draft material is not a `Thesis`
- only operator submission through the normal lifecycle workflow creates canonical thesis lifecycle facts

## Plan Workspace Import Flow

Plan import should be delayed until the Thesis Workspace import flow is stable.

Plan import is higher risk because it can touch execution-sensitive cognition. M14C should preserve the distinction between authored planning assistance and execution authority.

Plan import may eventually support:

- entry ideas
- target ideas
- risk notes
- invalidation ideas
- plan assumptions
- evidence references

Plan import must not support automatic:

- sizing
- approval
- execution authorization
- broker order creation
- lifecycle transition

Plan Workspace UX should require explicit operator validation of any imported plan-adjacent content. Sizing, approval, and authorization should remain explicitly operator-entered or operator-confirmed.

## Review Workspace Import Flow

Review import should support reflective cognition without turning imported reflections into operator truth.

Potential Review Workspace imports:

- journal reflections
- AI-assisted review suggestions
- evidence summaries
- behavioral observations
- lesson candidates

Review import must preserve:

- operator confirmation of lessons learned
- distinction between AI-generated reflection and operator-authored reflection
- replay-visible provenance
- non-canonical status before explicit review submission

Imported behavioral or emotional interpretations must not become automatic judgement. They remain advisory review inputs unless the operator explicitly incorporates them into a Review Artifact through normal review workflow.

## Thin Vertical Slice (TF-R001)

`TF-R001` should be the first M14C implementation slice.

Recommended scope:

```text
M12 advisory markdown/research artifact
    or deterministic Markdown/YAML thesis draft
    -> thesis import preview
    -> selective field acceptance
    -> operator edits
    -> manual thesis workflow submission
    -> replay-visible provenance
```

`TF-R001` should include:

- schema mapping for `thesis_draft`
- integration with existing M12 advisory artifact persistence where feasible
- thesis import preview UX
- field-level acceptance
- provenance indicators
- reject/defer path
- explicit manual thesis creation or revision
- traceability from resulting lifecycle event back to advisory source material

`TF-R001` should exclude:

- universal import framework
- watcher daemon
- background sync
- AI parsing
- plan import
- execution import
- automatic lifecycle event creation
- automatic conviction or sizing

The first slice should prove the canonical boundary and UX pattern before expanding scope.

## Implementation Sequencing

Recommended sequence:

```text
1. Confirm M12 advisory artifact substrate integration path
2. Define thesis_draft schema mapping
3. Design hybrid import preview UX
4. Implement TF-R001 thesis import preview and field acceptance
5. Add provenance markers and edit indicators
6. Add lifecycle promotion traceability
7. Verify replay reconstruction semantics
8. Operationally test boundary clarity
9. Prepare ADR candidate if the pattern stabilizes
10. Consider Plan and Review import extensions
```

Potential issue sequence:

- `TF-R001 - Thesis draft lifecycle import preview`
- `TF-R002 - Thesis import preview UX and field acceptance`
- `TF-R003 - Imported provenance overlays`
- `TF-R004 - Replay visibility for selective promotion`
- `TF-R005 - Plan workspace import support`

The exact issue IDs should be reconciled with the runtime issue register before implementation begins.

## Open Questions

- Should M14C reuse M12 advisory artifact persistence directly for all import drafts, or introduce a thin thesis-draft projection above it?
- What exact name should be used for the imported lifecycle-facing draft artifact?
- What minimum field-level provenance must be persisted in `TF-R001`?
- Should field acceptance be event-backed in the first slice?
- Should rejected or deferred imports remain replay-visible?
- How should operator edits after import affect source attribution?
- What is the final import state vocabulary?
- What visual language best distinguishes imported, accepted, edited, promoted, and canonical content?
- Should M14C design artifacts live in runtime `DOCS/design`, KB `knowledge/processed`, or both?
- When does this planning synthesis become ready for ADR preparation?

## Future Extensions

- Plan Workspace import support.
- Review Workspace reflection imports.
- Behavioral observation import support.
- Watchlist candidate import flow.
- Thesis evolution imports.
- Research Cockpit file contract refinement.
- Local filesystem watcher after manual import flow stabilizes.
- MCP ingestion after local and advisory artifact patterns stabilize.
- ADR candidate for operator cognition import and selective canonical promotion.
- M15 integration for replayable cognitive reconstruction.
- M16 integration for attention allocation and opportunity funnel workflows.

