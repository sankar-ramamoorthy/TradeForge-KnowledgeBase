---
title: TF-0031 React Frontend Scaffold Implementation Synthesis
type: processed-knowledge
status: processed
created: 2026-05-12
source: knowledge/raw/20260512-tf-0031-react-frontend-scaffold-implementation.md
issue: TF-0031
milestone: M7
---

# TF-0031 React Frontend Scaffold Implementation Synthesis

## Stabilized Outcome

TF-0031 established the React frontend runtime boundary without expanding into workspace routing, layout system, session identity, lifecycle workflow UI, or full MVP workspace implementation.

ADR 0021 now records that the frontend is an API consumer and interaction boundary, not an owner of canonical event truth, lifecycle authority, replay truth, workspace semantics, or projection internals.

## Durable Implementation Constraints

Future frontend issues should preserve these boundaries:

- Consume FastAPI endpoints rather than Python internals.
- Treat workspace data as derived read models.
- Keep browser state operational and temporary, not canonical.
- Preserve persona/workspace context explicitly when routing is added.
- Keep lifecycle actions routed through API/service boundaries.
- Keep replay views dependent on replay APIs and source references.

## Follow-On Issue Guidance

TF-0032 should add workspace routing within the ADR 0021 boundary. Routing may organize entrypoints, but it must not redefine workspace semantics or imply that routes are workspace authority.

TF-0033 should add shared operational layout primitives while preserving anti-dashboard UX doctrine and context-before-action hierarchy.
