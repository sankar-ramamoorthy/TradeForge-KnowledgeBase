---
title: M14C Research Cockpit Import Bridge Implementation Roadmap
type: implementation-roadmap
status: processed-draft
created: 2026-05-27
updated: 2026-05-27
tags:
  - TradeForge
  - M14C
  - implementation-roadmap
  - Import-Bridge
  - Research-Cockpit
  - replayability
sources:
  - "knowledge/processed/20260527-m14c-research-import-bridge-planning-synthesis.md"
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
  - [[Decision Lifecycle Engine]]
  - [[Event Ledger]]
  - [[Replay System]]
  - [[Advisory Artifact]]
---

# M14C Research Cockpit Import Bridge Implementation Roadmap

## Summary

This roadmap converts the M14C brainstorming synthesis into a cautious implementation sequence.

It remains exploratory. It does not establish canonical architecture or final event names.

## Runtime Baseline

M12 already completed the runtime foundation for Research Cockpit and advisory artifact ingestion:

- `TF-A016 - Define external research cockpit import boundary`
- `TF-A017 - Implement research artifact ingestion API`
- `TF-A018 - Implement Codex/Claude-generated advisory artifact support`
- `TF-A019 - Implement advisory markdown artifact persistence`
- `TF-A020 - Implement replay-safe advisory snapshot capture`

M14C implementation planning should start by reusing or extending those boundaries. It should not create a duplicate ingestion model unless the existing advisory artifact model cannot support thesis-draft lifecycle authorship.

## Milestone Goal

Introduce controlled ingestion of external research artifacts into TradeForge while preserving lifecycle discipline, canonical boundaries, replayability, provenance, and explicit operator approval.

## Non-Goals

- Universal import framework.
- Background watcher daemon.
- AI parsing.
- Plan import in the first slice.
- Execution import.
- Automatic thesis creation.
- Automatic plan creation.
- Automatic approval, conviction scoring, sizing, or execution authorization.

## Proposed Issue Sequence

### TF-R001 - Thesis Draft Filesystem Import

Goal:

Create the narrowest deterministic path from Markdown/YAML or existing advisory artifact input to non-canonical thesis draft preview.

Scope:

- Define `thesis_draft` schema version 1.
- Read files from configured import directory or map from existing M12 advisory markdown artifact persistence if that is the chosen integration path.
- Parse symbol, title, thesis narrative, catalysts, risks, assumptions, invalidation conditions, evidence links, and notes.
- Represent parsed material as a non-canonical imported draft/advisory artifact.
- Move parsed files to processed, rejected, or error folders only after explicit handling.
- Preserve source path and import timestamp in runtime-visible metadata.

Boundary:

- Must not emit `TradeThesisCreated` automatically.
- Must not infer approval, conviction, sizing, or execution intent.

### TF-R002 - Thesis Import Preview UX

Goal:

Expose imported thesis draft material in an operator-controlled preview flow.

Scope:

- Show imported fields before promotion.
- Support field-level acceptance.
- Support editing before lifecycle creation.
- Support reject path.
- Show provenance markers.
- Preserve visual distinction between imported content and canonical lifecycle content.

Open UX decision:

- Dedicated Import Review workspace versus embedded Thesis workspace preview versus hybrid pattern.

### TF-R003 - Imported Provenance Overlays

Goal:

Make imported origin and operator edits visible in operational surfaces.

Scope:

- Display source file, source system, import timestamp, and schema version where relevant.
- Mark fields accepted from import.
- Mark fields edited after import.
- Preserve source references for later replay and review.

Boundary:

- Provenance overlays are derived/read-model presentation unless an explicit event design is approved.

### TF-R004 - Replay Visibility For Imports

Goal:

Ensure replay can reconstruct the fact that imported research influenced lifecycle authorship.

Scope:

- Show source provenance for lifecycle material promoted from imports.
- Distinguish imported advisory material from canonical lifecycle events.
- Avoid replay dependency on live filesystem paths or external APIs.

Open architecture decision:

- Determine whether `TF-R004` can rely on the completed M12 replay-safe advisory snapshot capture, or whether lifecycle-import-specific replay metadata is required.

### TF-R005 - Plan Workspace Import Support

Goal:

Extend import support to plan-related material only after thesis import is stable.

Scope:

- Import entries, targets, risks, and invalidation ideas as non-canonical plan draft material.
- Preserve explicit operator validation.

Hard boundaries:

- No imported sizing authority.
- No imported approval authority.
- No imported execution authorization.

## Suggested Sequencing

```text
M14C design spec
    -> schema v1
    -> TF-R001 parser/import draft model
    -> TF-R002 preview UX
    -> TF-R003 provenance overlays
    -> TF-R004 replay visibility
    -> operational testing
    -> ADR candidate
    -> TF-R005 plan import only if boundaries hold
```

## Acceptance Criteria For M14C Thin Slice

- A Markdown/YAML thesis draft can be imported from a configured local directory.
- Imported material is represented as non-canonical draft/advisory material.
- The operator can review, edit, reject, or promote imported material.
- Promotion uses normal lifecycle workflow and canonical lifecycle events.
- The importer does not write canonical lifecycle events directly.
- Source provenance remains visible after promotion.
- Replay can distinguish imported advisory source material from canonical lifecycle facts.

## Required Design Decisions Before Code

- External artifact schema v1.
- Import draft artifact name and ownership boundary.
- Import state vocabulary.
- File movement conventions.
- Preview UX location.
- Provenance fields.
- Whether M14C reuses M12 advisory artifact persistence directly or introduces a separate thesis-draft import model.

## ADR Candidate Later

An ADR may be appropriate after `TF-R001` through `TF-R004` are tested.

Candidate ADR scope:

- External research ingestion boundary.
- Advisory artifact versus canonical lifecycle event distinction.
- Provenance requirements for replay.
- Prohibited import behaviors.
- UX pattern for selective acceptance and operator promotion.
