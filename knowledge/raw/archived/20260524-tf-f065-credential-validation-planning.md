---
title: TF-F065 Credential Validation And Test Workflow Planning
type: raw-planning
status: raw
created: 2026-05-24
source_issue: TF-F065
milestone: M13A
tags:
  - TradeForge
  - M13A
  - credential-validation
  - runtime-planning
---

# TF-F065 Credential Validation And Test Workflow Planning

## Issue Discipline

- Issue ID: TF-F065
- Milestone: M13A
- Title: Implement Credential Validation And Test Workflow
- Affected layers: security, app, frontend
- Linked ADRs: ADR-0037, ADR-0038
- Impacted invariants: Derived State Must Remain Distinguishable, Historical Integrity, Architectural Simplicity
- Depends on: TF-F060, TF-F063, TF-F064

## Design Reasoning

Credential management currently saves, masks, rotates, revokes, and reloads providers. Operators cannot distinguish a configured-but-untested credential from a structurally invalid credential or a credential that was explicitly validated. TF-F065 should add a bounded validation/test workflow without introducing external secret managers or canonical event-ledger truth.

For M13A, validation should be safe and deterministic by default:

- It must not expose plaintext secrets.
- It must not write canonical events.
- It should update credential metadata (`last_validated_at`, status) in the encrypted credential store.
- It should distinguish missing, revoked, untested, active/configured, invalid, and unknown/expired states.
- Initial validation can be structural and adapter-boundary aware; live provider calls can remain future/deeper diagnostics unless already safe and local.

## Event, Lifecycle, And Replay Impact

- No new event types.
- No lifecycle transitions.
- Credential validation metadata is operational/security state, not event-ledger truth.
- Replay should not call provider validation endpoints to reconstruct historical credentials.

## Implementation Plan

1. Add an `INVALID` credential status to the security credential model.
2. Add `POST /admin/credentials/{provider_id}/validate` for explicit operator-triggered validation.
3. Implement validation by decrypting the saved credential, checking required fields, rejecting revoked/missing credentials, and persisting active or invalid metadata with `last_validated_at`.
4. Return safe status metadata only; never return plaintext secrets.
5. Update provider governance credential status mapping to include invalid state.
6. Add focused tests for successful validation, missing/revoked provider errors, invalid encrypted payload handling, status persistence, and no secret leakage.
7. Sync issue register and roadmap after verification.

## ADR Checkpoint

No new ADR is required. ADR-0037 already defines the operational credential boundary and local encrypted credential store. TF-F065 adds a bounded validation workflow inside that boundary without changing credential authority or lifecycle semantics.
