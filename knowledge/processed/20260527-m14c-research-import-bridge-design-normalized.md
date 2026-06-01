---
title: M14C Research Import Bridge Design Exploration
type: brainstorming-note
status: processed-draft
created: 2026-05-27
updated: 2026-05-27
tags:
  - TradeForge
  - M14C
  - Research-Cockpit
  - Import-Bridge
  - UX-doctrine
  - provenance
  - replayability
source:
  - "knowledge/raw/M14C - research-import-bridge-design.md"
related_milestones:
  - M14C
related_issues:
  - TF-A016
  - TF-A017
  - TF-A018
  - TF-A019
  - TF-A020
  - TF-R001
related:
  - [[Persona Workspace]]
  - [[Workspace Surface]]
  - [[Advisory Artifact]]
  - [[Decision Lifecycle Engine]]
---

# M14C Research Import Bridge Design Exploration

## Summary

This note argues that the Research Cockpit Import Bridge should be designed before implementation because the import experience crosses Thesis, Plan, Review, Replay, and later behavioral/reflection workspaces.

The note frames the work as cross-workspace interaction doctrine rather than a narrow file import feature.

The central design statement is:

> External research may assist lifecycle cognition, but lifecycle intent, authorization, and canonicalization remain explicitly operator-controlled.

## Runtime Alignment

The runtime roadmap and issue register already record completed M12 work for the lower-level advisory artifact boundary:

- `TF-A016 - Define external research cockpit import boundary`
- `TF-A017 - Implement research artifact ingestion API`
- `TF-A018 - Implement Codex/Claude-generated advisory artifact support`
- `TF-A019 - Implement advisory markdown artifact persistence`
- `TF-A020 - Implement replay-safe advisory snapshot capture`

This note should be interpreted as exploring the cross-workspace UX, lifecycle promotion, and provenance interaction model above that foundation.

## Key Concepts

- TradeForge is not intended to be the primary long-form writing environment.
- Operators may use ChatGPT, Claude, Codex, research notes, Markdown documents, and other external AI tooling.
- TradeForge should ingest structured reasoning without surrendering canonical authority.
- The import experience must remain coherent across lifecycle workspaces.
- The design should precede ADR creation; an ADR should wait until interaction shape stabilizes.

## UX Ideas

### Preserve Existing Lifecycle Entry

The operator still explicitly enters or confirms Thesis, Plan, Review, and other lifecycle materials.

### Import Assists Authorship

Importing supports reasoning construction but does not replace operator authorship or approval.

### Selective Acceptance

The operator may accept:

- individual fields
- grouped suggestions
- entire sections

### Manual Risk Ownership

Certain fields should remain explicitly operator-authored or operator-confirmed:

- conviction
- sizing
- approval
- authorization
- execution authority

## Competing UX Options

### Option A: Dedicated Import Review Workspace

Imported material lands in an Import Review workspace before entering lifecycle-specific surfaces.

Benefits:

- Strong canonical boundary.
- Clear operator review step.
- Easier rejection/error handling.

Risks:

- Could add navigation overhead.
- Could fragment cognition if the operator expects to work inside Thesis or Plan surfaces.

### Option B: Embedded Lifecycle Import Preview

Imported material appears directly inside Thesis, Plan, Review, or Replay workspaces as preview material.

Benefits:

- Keeps review near the target lifecycle context.
- May feel more natural for field-level acceptance.

Risks:

- Boundary between draft material and lifecycle material may become visually ambiguous.
- Plan import is especially risky if imported sizing, approval, or execution content appears too close to canonical workflow controls.

### Option C: Hybrid Pattern

Use a common import review model, but render it in the lifecycle workspace where the material is relevant.

Benefits:

- Preserves a consistent mental model across workspaces.
- Allows field-level acceptance while preserving explicit review state.

Risks:

- Requires careful UX discipline to avoid hidden promotion semantics.

## Architectural Considerations

- Imported artifacts require provenance fields such as source file, import timestamp, source system, optional source model/provider, and operator edits after import.
- Imported content may require non-canonical import events or advisory capture facts, but canonical lifecycle events should occur only after promotion.
- A primitive external artifact schema should be defined early to prevent schema drift.

Potential non-canonical/import events named in the note:

- `ResearchDraftImported`
- `ImportedFieldAccepted`
- `ImportedDraftRejected`

Potential canonical events after promotion:

- `ThesisCreated`
- `PlanStructured`
- `ReviewRecorded`

These names remain exploratory and should be reconciled with the existing event taxonomy before runtime implementation.

## Canonical Boundary Concerns

- External artifacts must not directly emit canonical lifecycle events.
- Imported Plan material is higher risk than imported Thesis material because it may contain execution-sensitive fields.
- Replay should show imported material as advisory overlay or provenance context, not as retroactive canonical truth.
- Behavioral/reflection imports must not become automatic judgement or auto-generated operator assessment.

## Stable Architectural Direction

- Design spec first, thin implementation second, ADR later after operational testing.
- The import model should be cross-workspace, not a one-off file utility.
- Provenance and replay semantics are part of the core design, not later enhancements.
- Imported content remains advisory until explicit operator action.

## Unresolved Design Questions

- Should the design spec live in runtime docs, the KB, or both during exploration?
- Which event names are appropriate for imported draft state?
- Should field-level acceptance be event-backed in the first slice?
- How should operator edits after import be represented in replay?
- What is the visual language for imported but non-canonical content?

## Risky Assumptions

- A shared UX pattern can cover Thesis, Plan, Review, Replay, and Behavioral workspaces without over-generalizing too early.
- A canonical external artifact schema can be defined early without freezing the import model prematurely.
- Operators will understand the difference between imported suggestions and canonical lifecycle material if the UI presents provenance clearly.

## Doctrinal Implications

- UX is architectural because import review affects decision quality and boundary clarity.
- Human Decision Sovereignty requires selective acceptance and explicit promotion.
- Event-sourcing doctrine requires careful distinction between import facts, advisory artifacts, and lifecycle events.
- Replayability requires provenance and edit history to remain visible.

## Open Questions

- How should imported fields display provenance markers?
- Should accepted imported fields retain source-level attribution after editing?
- What fields must be prohibited from automated import?
- What does "manual validation" mean for plan imports?

## Future Extensions

- Research Cockpit integration.
- AI-generated trade candidates.
- Advisor feed ingestion.
- Watchlist imports.
- Thesis evolution imports.
- Structured Markdown contracts.
- Local filesystem watchers.
- Eventual MCP ingestion.
