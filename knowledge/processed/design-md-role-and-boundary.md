---
title: DESIGN.md Role And Boundary
type: processed-synthesis
status: processed
tags: [TradeForge, design-doc, UX, workflow, behavioral-architecture, semantic-governance]
created: 2026-05-09
updated: 2026-05-09
source_history:
  - knowledge/raw/initial brainstorm about design.md
  - knowledge/raw/brainstorm-20260509-tradeforge-design-md.md
  - knowledge/raw/when would i introduce design.md processed separately in [[DESIGN.md Introduction Timing]]
related:
  - "[[Behavioral And Experiential Architecture]]"
  - "[[DESIGN.md Introduction Timing]]"
  - "[[UX_DOCTRINE]]"
  - "[[WORKSPACES]]"
  - "[[ARCHITECTURE]]"
  - "[[INVARIANTS]]"
  - "[[Runtime KB Development Loop]]"
---

# DESIGN.md Role And Boundary

## Status

Processed synthesis from an initial brainstorm about whether TradeForge should have a `DESIGN.md` document.

This note is not canonical product doctrine. It stabilizes the architectural question and identifies a likely documentation boundary for future promotion.

The source raw notes were processed and removed after this synthesis:

- `knowledge/raw/initial brainstorm about design.md`
- `knowledge/raw/brainstorm-20260509-tradeforge-design-md.md`

## Core Synthesis

TradeForge likely needs a behavioral and experiential design layer that is distinct from existing architecture, UX, workspace, ADR, and runtime implementation documents.

The proposed purpose of `DESIGN.md` is:

```text
the behavioral and experiential architecture of the TradeForge product
```

This means `DESIGN.md` would explain how the system behaves from the operator's perspective:

- workflow experience
- workspace cognition
- lifecycle interaction
- decision-state visibility
- AI interaction patterns
- operational rhythm
- trust and auditability
- persona-aware behavior
- review and reflection experience

The document should not become:

- a generic engineering design document
- a random brainstorm dump
- a replacement for ADRs
- a replacement for [[UX_DOCTRINE]]
- a replacement for [[WORKSPACES]]
- an implementation plan

## Ontology Implications

The brainstorm introduces a candidate semantic layer: behavioral and experiential architecture.

This layer is not yet canonical, but it appears useful as a bridge between:

- [[ARCHITECTURE]]: structural system design
- [[UX_DOCTRINE]]: interaction philosophy and cognitive UX principles
- [[WORKSPACES]]: canonical workspace ontology
- runtime docs: implementation-facing behavior and components

The candidate ontology relationship is:

```text
Architecture Doctrine
    -> UX Doctrine
    -> Workspace Ontology
    -> Behavioral And Experiential Architecture
    -> Runtime Design Translation
```

Important ontology boundary:

- [[UX_DOCTRINE]] defines interaction philosophy.
- [[WORKSPACES]] defines workspace concepts.
- `DESIGN.md` would define intended product behavior and experiential sequencing.
- runtime `DOCS/design.md` would translate that behavior into implementation-facing guidance.

Potential terminology concern:

- "behavioral architecture"
- "experiential architecture"
- "behavioral and experiential architecture"

These terms should remain draft-level until promoted through a canonical glossary or doctrine update.

## Workflow Implications

A future `DESIGN.md` should be created through progressive stabilization, not directly from brainstorm content.

Recommended workflow:

```text
raw brainstorm
    -> processed synthesis
    -> draft topic page
    -> DESIGN.md outline
    -> canonical DESIGN.md only after doctrine alignment
    -> runtime DOCS/design.md translation when implementation surfaces exist
```

Workflow implications:

- KB-level design doctrine should precede runtime design translation.
- runtime `DOCS/design.md` should reference the KB authority rather than redefine product behavior.
- design guidance should be checked against invariants, workspace doctrine, UX doctrine, AI governance, and replayability.
- examples such as "Market Scanning Workspace" or "Execution Workspace" should not be promoted as canonical workspace names without alignment against [[WORKSPACES]].

## Playbook Implications

The [[Runtime KB Development Loop]] may eventually need guidance for design-doctrine work.

Candidate playbook rules:

- behavioral design changes should start in the KB, not runtime docs.
- runtime design docs should translate stable KB doctrine into implementation-facing guidance.
- design-doc updates should call out whether they affect ontology, UX doctrine, workspace semantics, lifecycle behavior, AI boundaries, or replay expectations.
- `DESIGN.md` should not absorb ADR responsibilities or implementation planning responsibilities.
- dual KB/runtime design documents require explicit authority boundaries to avoid drift.

No immediate playbook rewrite is required. This synthesis preserves the guidance for future playbook refinement.

## Doctrine Integration Opportunities

The brainstorm reinforces existing doctrine:

- [[INVARIANTS]]: UX is architectural; workflow integrity, human judgment, and reflection are core.
- [[UX_DOCTRINE]]: context before action, cognitive continuity, visible uncertainty, and review-first design.
- [[WORKSPACES]]: workspaces are cognitive and operational environments, not dashboards or tabs.
- [[ARCHITECTURE]]: UI is a projection of workspace state; the system is workflow-centric and decision-first.

Potential future integration:

- add a KB-level `DESIGN.md` as a stable doctrine document after more synthesis.
- add a runtime `DOCS/design.md` only after implementation-facing behavior needs a translation layer.
- add a topic or glossary entry for behavioral and experiential architecture if the term stabilizes.
- use [[DESIGN.md Introduction Timing]] to defer canonical `DESIGN.md` until M5 readiness conditions exist.

## Cross-Linking Opportunities

Stable cross-links created or recommended:

- [[Behavioral And Experiential Architecture]]
- [[UX_DOCTRINE]]
- [[WORKSPACES]]
- [[ARCHITECTURE]]
- [[INVARIANTS]]
- [[Runtime KB Development Loop]]

Potential future links:

- `DESIGN.md`
- runtime `DOCS/design.md`
- workspace map
- UX map
- workflow map

## ADR Implications

No runtime ADR is required now.

An ADR may become appropriate if TradeForge formally adopts:

- a KB-level `DESIGN.md` as canonical doctrine
- a runtime `DOCS/design.md` translation layer
- a new authority relationship between UX doctrine, workspace doctrine, and runtime design documentation

Until then, this remains a KB stabilization topic rather than a runtime architecture decision.

## Operational Synchronization Findings

No runtime issue, roadmap, branch, or implementation state needs to change from this processing pass.

The brainstorm concerns documentation doctrine and product behavior semantics, not completed runtime implementation.

## Open Questions

- Should `DESIGN.md` become canonical doctrine or first exist as a draft topic-derived outline?
- How should it differ from [[UX_DOCTRINE]] without duplicating it?
- Should runtime `DOCS/design.md` exist now or wait until more runtime surfaces are implemented?
- Should "behavioral and experiential architecture" become a glossary term?
- Which document should own product-level anti-patterns: `DESIGN.md`, [[UX_DOCTRINE]], or a playbook?

## Contradictions

No contradiction with current doctrine was identified.

The main risk is premature canonicalization: creating `DESIGN.md` too early could duplicate existing doctrine or freeze unstable terminology.

## Processing Result

Stable knowledge was promoted to:

- `knowledge/processed/design-md-role-and-boundary.md`
- `knowledge/topics/behavioral-and-experiential-architecture.md`

The source raw notes were deleted after traceable promotion:

- `knowledge/raw/initial brainstorm about design.md`
- `knowledge/raw/brainstorm-20260509-tradeforge-design-md.md`
