---
title: TF-F028 To TF-F042 Planning - Feedback Cluster Registration
type: brainstorm
status: raw
tags: [tradeforge, planning, feedback-cluster, opportunity-workspace, ux]
created: 2026-05-17
---

# TF-F028 To TF-F042 Planning - Feedback Cluster Registration

## Approach

- Register the Opportunity Workspace feedback as a cluster of bounded `TF-F###` issues rather than one omnibus issue.
- Preserve the original feedback source and the structured brainstorm note as traceability anchors.
- Keep all issues in `Planned` state because this pass is for identification only.
- Record richer issue detail now so later implementation passes can choose a single bounded issue without having to rediscover the original reasoning.

## Decomposition Rationale

- `TF-F028` to `TF-F036` cover local UX and workflow-surface gaps that can likely be handled incrementally.
- `TF-F037`, `TF-F038`, `TF-F039`, and `TF-F040` capture architectural or doctrinal discoveries that require explicit future design before implementation.
- `TF-F041` and `TF-F042` capture synthesis-layer follow-ons that depend on how interpretation work is eventually bounded.

## Replay / Lifecycle Impact

- No direct event, replay, or lifecycle changes are introduced in this triage pass.
- Several future issues must preserve advisory boundaries and replay-visible provenance when they are eventually designed.

## Open Questions

- Which issues should be promoted into milestone work versus handled as ordinary feedback fixes?
- Should `TF-F037` and `TF-F038` become milestone-level architecture work after diagnosis?
- Should `TF-F039` and `TF-F040` become doctrine updates before dependent UI issues proceed?
- Which local UX issues can move immediately without waiting on the larger interpretation architecture?

## Recommended Resolution Order

### Foundation First

1. `TF-F040` - define the trader-language boundary between canonical ontology and operator-facing UX.
2. `TF-F039` - define recovery-oriented missing-information behavior before multiplying new context surfaces.
3. `TF-F037` - design the context interpretation layer that translates provider payloads into operator cognition.

### Main Requirement Path

4. `TF-F038` - define the dedicated Context Workbench workspace concept.
5. `TF-F032` - define the explicit advisory context acquisition workflow the Workbench will own.
6. `TF-F033` - expose provider-attempt transparency, fallback outcomes, and retrieval provenance.
7. `TF-F034` - distinguish equity fundamentals from ETF-relevant context.

### Downstream Productization

8. `TF-F041` - connect acquired advisory context into opportunity synthesis and thesis implications.
9. `TF-F042` - make market-context presentation interpretation-first rather than raw-payload-first.
10. Local Opportunity Workspace refinement issues:
    - `TF-F028`
    - `TF-F029`
    - `TF-F030`
    - `TF-F031`
    - `TF-F035`
    - `TF-F036`

## Why This Order

- `TF-F038` should not be treated as "add another screen." It is a workspace-architecture decision.
- `TF-F037` should precede `TF-F038` so the Workbench is designed around the correct role for interpretation, not just retrieval.
- `TF-F040` and `TF-F039` should come first so the new workspace does not inherit the same ontology-leakage and dead-end empty-state problems that the feedback session exposed.
- `TF-F032`, `TF-F033`, and `TF-F034` are the operational backbone that makes the Workbench trustworthy once its concept is accepted.
- `TF-F041` and `TF-F042` are downstream because they depend on what context exists and how the interpretation layer is bounded.

## Milestone Assignment

The Opportunity Workspace feedback cluster is now assigned to:

```text
M10E - Context Workbench And Advisory Context Acquisition
```

`M10E` is positioned after `M10D` and before `M11`.

Reasoning:

- `M10D` made provider capability explicit.
- `M10E` turns that provider foundation into a coherent operator-facing research and context-acquisition layer.
- `M11` should then build AI advisory on top of a clear acquisition and interpretation model rather than inventing that missing layer during AI work.
