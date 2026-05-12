---
title: TF-0031 React Frontend Runtime Boundary Planning Synthesis
type: processed-knowledge
status: processed
created: 2026-05-12
source: knowledge/raw/20260512-tf-0031-react-frontend-scaffold-planning.md
issue: TF-0031
milestone: M7
---

# TF-0031 React Frontend Runtime Boundary Planning Synthesis

## Summary

TF-0031 should establish a React/TypeScript frontend runtime boundary without implementing full workspace routing, workspace screens, authentication, or lifecycle workflows.

The frontend is a projection consumer and interaction boundary. It must consume FastAPI contracts rather than importing runtime internals or treating client state as canonical truth.

## Stabilized Constraints

- React runtime belongs to the frontend layer.
- Workspace semantics remain governed by KB doctrine and runtime ADRs.
- Workspaces must not be reduced to pages, tabs, or dashboards.
- The scaffold may include a minimal workspace shell, but full routing belongs to TF-0032 and layout primitives belong to TF-0033.
- ADR 0021 is required because the issue register already depends on it and the ADR file is missing.

## Implementation Boundary

Acceptable for TF-0031:

- `frontend/` project scaffold
- TypeScript configuration
- Vite React entrypoint
- command documentation
- ADR 0021 creation
- issue register synchronization

Out of scope for TF-0031:

- lifecycle action UI
- workspace routing system
- shared operational layout system
- session/auth model
- direct event ledger access
- persistent client-side workspace truth

## Replayability Note

Future frontend replay and workspace views must consume deterministic API outputs and preserve source references. The scaffold should not introduce browser state as historical truth.
