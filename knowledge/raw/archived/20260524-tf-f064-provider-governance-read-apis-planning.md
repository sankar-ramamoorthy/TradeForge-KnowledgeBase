---
title: TF-F064 Provider Governance Read APIs Planning
type: raw-planning
status: raw
created: 2026-05-24
source_issue: TF-F064
milestone: M13A
tags:
  - TradeForge
  - M13A
  - provider-governance
  - runtime-planning
---

# TF-F064 Provider Governance Read APIs Planning

## Issue Discipline

- Issue ID: TF-F064
- Milestone: M13A
- Title: Implement Provider Governance Read APIs
- Affected layers: app, services, security
- Linked ADRs: ADR-0032, ADR-0037, ADR-0038, ADR-0042
- Impacted invariants: Derived State Must Remain Distinguishable, AI Advisory Boundary, Human Decision Sovereignty, Architectural Simplicity

## Design Reasoning

The runtime already exposes narrow provider configuration under `/workspaces/provider-configuration` and credential administration under `/admin/credentials`. M13A needs a read model for the future provider governance surface without moving administration into contextual rails.

The implementation should add a read-only provider governance endpoint that composes existing runtime state:

- Provider registry descriptors and supported capabilities.
- Capability resolution for deterministic preferred-plus-fallback routing.
- Credential status from CredentialStore without plaintext, masked, or display field values.
- AI gateway status derived from the configured advisory provider and LiteLLM credential presence.
- Diagnostic summary limited to ephemeral/current operational status, not retained health history.
- Authority metadata that marks the response advisory/operational and non-canonical.

## Event, Lifecycle, And Replay Impact

- No event types are added.
- No event ledger writes occur.
- No lifecycle transition authority is introduced.
- Replay must not call live providers to reconstruct provider health; this endpoint reports current operational state only.
- Provider and credential state remain external-system governance state, not canonical decision truth.

## Implementation Plan

1. Add Pydantic response contracts to `src/app/api/routes.py` for provider governance overview, providers, credentials, routes, diagnostics, and AI gateway status.
2. Add helper composition logic that reuses the existing provider registry and credential store.
3. Add `GET /provider-governance` on the runtime router as an app-level governance read endpoint, not a workspace route.
4. Preserve existing `/workspaces/provider-configuration` behavior unchanged.
5. Add focused API tests verifying route resolution, credential status without secrets, AI gateway metadata, authority flags, and non-mutation of the event store.
6. Update roadmap/register after verification.

## ADR Checkpoint

No new ADR is required for TF-F064. The governing architecture is already established by ADR-0032, ADR-0037, ADR-0038, ADR-0042 and M13A design docs TF-F060 through TF-F063. This implementation adds a bounded read model under that existing architecture.
