---
title: First Operational MVP Vertical Slice
type: topic
status: draft
tags:
  - TradeForge
  - topic
  - M8
  - workspace
  - replay
  - lifecycle
  - frontend
  - operational-cognition
created: 2026-05-13
updated: 2026-05-13
source:
  - knowledge/processed/plan-20260512-tf-0035-operating-workspace.md
  - knowledge/processed/implemented-20260512-tf-0035-operating-workspace.md
  - knowledge/processed/plan-20260512-tf-0036-opportunity-workspace.md
  - knowledge/processed/implemented-20260512-tf-0036-opportunity-workspace.md
  - knowledge/processed/implemented-20260513-tf-0037-through-tf-0041-m8-operational-mvp-vertical-slice.md
related:
  - "[[Workspace]]"
  - "[[Projection]]"
  - "[[ReplayTimeline]]"
  - "[[ReviewArtifact]]"
  - "[[Persona Workspace Projection Layer]]"
---

# First Operational MVP Vertical Slice

## Definition

The First Operational MVP Vertical Slice is the first runtime milestone where TradeForge behaves as
a usable operational cognition system rather than a set of bounded infrastructure capabilities.

It is the point where:

- six core Persona Workspace surfaces are implemented
- user-facing lifecycle progression is operational
- replay is visible inside the same runtime experience
- review is event-backed and reachable from the same workflow chain

## Stabilized M8 Result

M8 established these operational surfaces:

- Operating Workspace
- Opportunity Workspace
- Plan Review Workspace
- Active Position Workspace
- Replay Workspace
- Review Workspace

Together they form the first complete discretionary workflow path from pre-decision cognition
through reflective learning.

## Stable Interaction Pattern

The vertical slice stabilized a repeatable frontend pattern for TradeForge workspaces:

```text
workspace projection context
    -> authority-labeled field surfaces
    -> stage-gated lifecycle action surface (optional)
    -> explicit authority boundary notes
    -> source-linked metadata
```

Replay adds one bounded extension:

```text
workspace projection context
    + replay timeline reconstruction
    -> read-only historical cognition surface
```

## Authority Boundary Result

M8 did not collapse frontend behavior into workflow authority.

Preserved boundaries:

- lifecycle state remains owned by the Decision Lifecycle Engine
- workspace state remains derived from projections
- replay remains deterministic and read-only
- review completion remains event-backed
- UI actions request transitions; they do not create canonical state by themselves

This is the main architectural success condition of the vertical slice.

## Relation To M6

M6 established the [[Persona Workspace Projection Layer]] as deterministic workspace context.

M8 validated that this layer was sufficient to support:

- operational attention routing
- opportunity development
- plan authorization
- position supervision
- replay reconstruction surfaces
- review completion flow

The projection layer is therefore no longer only a draft runtime foundation. It has now been
proven in full workspace usage.

## Relation To Replayability

The vertical slice is significant because replay is not a sidecar analytics feature.

Replay participates directly in the MVP workflow:

- lifecycle transitions append canonical events
- workspace surfaces consume derived projections of that event history
- replay reconstructs the same history as an operator-facing surface
- review closes the loop as a durable workflow outcome

This confirms the TradeForge claim that replay and review are first-class, not optional extras.

## Post-M8 Boundary

M8 does not introduce:

- market context intelligence automation
- scenario discovery automation
- AI decision authority
- live broker execution
- post-MVP adaptive systems

Those remain deferred to the post-MVP roadmap checkpoint.
