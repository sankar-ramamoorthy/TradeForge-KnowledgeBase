---
title: Implemented TF-0015 Workspace State Contracts
type: raw-implementation-capture
status: raw
created: 2026-05-10
related:
  - TF-0015
  - ADR 0004
  - ADR 0008
  - ADR 0012
  - ADR 0013
---

# Implemented TF-0015 Workspace State Contracts

## Implementation Summary

Runtime now includes immutable workspace state contracts for the ADR 0012 workspace set.

Added service-layer contracts under `src/services/workspace_engine/contracts.py`:

- `WorkspaceStateAuthority`
- `WorkspaceStateField`
- `WorkspaceLifecycleAction`
- `WorkspaceReplayRequirement`
- `WorkspaceStateContract`
- `WorkspaceStateContractCatalog`
- `UnknownWorkspaceStateContractError`

The catalog covers:

- Operating Workspace
- Opportunity Workspace
- Plan Review Workspace
- Active Position Workspace
- Replay Workspace
- Review Workspace
- Market Context Workspace
- Playbooks / Doctrine Workspace

## Boundary Preserved

Contracts describe derived read-model shape and authority boundaries only.

They do not:

- append events
- mutate lifecycle state
- execute projections
- persist workspace state
- call infrastructure adapters
- implement frontend or API behavior

Each field is classified as canonical, derived, inferred, or advisory. Canonical fields are source references only, not workspace-owned truth.

## Verification

Runtime validation completed:

- `uv run pytest`
- `uv run ruff check .`
- `uv run mypy src tests`

## Operational Sync

Runtime issue state was updated:

- `DOCS/ISSUE_REGISTER.md` marks TF-0015 as Done and clarifies the eight-workspace ADR 0012 scope.
- `DOCS/Milestone_Roadmap_v2.md` marks TF-0015 as Done within M4.

## Follow-Up Boundary

M4 runtime issues are complete. Future workspace projection work should consume these contracts without treating contract declarations as canonical state or projection storage.
