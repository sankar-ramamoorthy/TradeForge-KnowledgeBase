---
title: TF-F067 Provider Governance Frontend Planning
type: raw-planning
status: raw
created: 2026-05-24
source_issue: TF-F067
milestone: M13A
tags:
  - TradeForge
  - M13A
  - provider-governance
  - frontend-planning
---

# TF-F067 Provider Governance Frontend Planning

## Issue Discipline

- Issue ID: TF-F067
- Milestone: M13A
- Title: Implement Provider Governance Frontend Surface And Rail Cleanup
- Affected layer: frontend
- Linked ADRs: ADR-0021, ADR-0032, ADR-0037, ADR-0038, ADR-0042
- Impacted invariants: UX Is Architectural, Workspaces Are Operational Environments, Workflow-Centric Architecture, Derived State Must Remain Distinguishable
- Depends on: TF-F060, TF-F064, TF-F065, TF-F066

## Design Reasoning

The existing `ProviderConfigurationPanel` appears in contextual rails and includes credential administration. M13A requires long-form provider governance to move into a dedicated external-systems surface while contextual rails remain short status/provenance/fallback surfaces.

Implementation should:

- Add a reachable Provider Governance surface from the shell navigation.
- Keep the surface outside canonical decision/lifecycle authority.
- Reuse existing credential administration and provider configuration controls on that dedicated surface.
- Add AI Gateway and Diagnostics sections from the read APIs.
- Replace right-rail administration with a compact provider status rail and configure link.
- Preserve decision workspaces and lifecycle flows unchanged.

## Event, Lifecycle, And Replay Impact

- Frontend-only surface; no event model changes.
- No lifecycle transition actions.
- Provider governance UI displays operational/advisory state only.
- Replay behavior unchanged.

## Implementation Plan

1. Add a Provider Governance route to shell navigation with explicit external-systems language.
2. Add `ProviderGovernanceWorkspace` using provider governance AI gateway data, existing credential/configuration controls, diagnostics, and authority boundaries.
3. Add a compact `ProviderStatusRail` for decision context rails with selected provider/fallback/gateway status and a configure link.
4. Remove `ProviderConfigurationPanel` from contextual rails.
5. Add frontend API types/helpers as needed for the provider governance overview.
6. Verify with `npm.cmd run typecheck` and backend checks already established.

## ADR Checkpoint

No new ADR is required. This implements the TF-F060 control-surface design and existing UX/runtime ADRs without changing workspace semantics or lifecycle authority.
