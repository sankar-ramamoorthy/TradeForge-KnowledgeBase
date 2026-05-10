---
title: TF-0015 Workspace State Contracts Plan
type: raw-planning-capture
status: raw
created: 2026-05-10
related:
  - TF-0015
  - ADR 0004
  - ADR 0007
  - ADR 0008
  - ADR 0012
  - ADR 0013
---

# TF-0015 Workspace State Contracts Plan

## Issue Scope

Runtime issue TF-0015 defines workspace state contracts for Milestone 4.

The issue is limited to contract definitions for derived workspace read models. It must not implement projection execution, persistent projection storage, frontend UI, API endpoints, event appending, or lifecycle mutation.

## Workspace Set

The contract catalog should align with ADR 0012 and the TF-0014 route catalog:

- Operating Workspace
- Opportunity Workspace
- Plan Review Workspace
- Active Position Workspace
- Replay Workspace
- Review Workspace
- Market Context Workspace
- Playbooks / Doctrine Workspace

This intentionally expands the TF-0015 issue text from six named workspaces to the eight-workspace ADR 0012 set so runtime contracts and routing remain aligned.

## Semantic Alignment

Relevant KB doctrine:

- Workspaces are persona-scoped operational decision environments.
- Workspace state is derived operational state, not canonical truth.
- Workspace projections consume event ledger history, lifecycle state, market intelligence, scenario outputs, and execution feedback.
- Replay must reconstruct briefing, opportunity, exposure, decision queue, and review state.
- Derived, inferred, and advisory state must remain distinguishable from canonical event-backed truth.

## Runtime Design

Add immutable workspace contract models under the workspace engine service boundary.

Each contract should identify:

- route id
- operational question
- state fields
- allowed lifecycle-aware actions
- required event inputs
- replay requirements
- authority boundaries

Each field should classify its authority as canonical, derived, inferred, or advisory.

## Event Impact

No new events are introduced.

The contracts may name required event domains or event types as inputs, but they do not create event semantics or append to the ledger.

## Lifecycle Impact

No lifecycle stage changes are introduced.

Allowed lifecycle-aware actions are contract declarations only. They must route future behavior through lifecycle services and must not mutate lifecycle state directly.

## Replay Impact

Contracts make replay inputs explicit by identifying source event requirements and reconstruction responsibilities for each workspace.

## Testing Strategy

Tests should verify:

- every ADR 0012 route has exactly one contract
- contracts contain state fields with explicit authority classification
- contracts include required event inputs and replay requirements
- no contract owns canonical lifecycle truth
- contract modules avoid infrastructure, app, event-store, and lifecycle orchestration dependencies

## ADR Checkpoint

No new ADR is required if implementation remains a lean contract layer under ADR 0012 and ADR 0013. A new ADR would be required if contracts introduce new workspace semantics, projection execution behavior, or lifecycle authority.
