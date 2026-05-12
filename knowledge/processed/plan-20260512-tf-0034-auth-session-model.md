---
title: Plan - TF-0034 Authentication/Session Model
type: processed-planning
status: processed
created: 2026-05-12
source: runtime-sources (no raw KB note captured; synthesized from runtime ISSUE_REGISTER and ADR 0022)
issue: TF-0034
milestone: M7
branch: feature/tf-0034-auth-session-model
related:
  - "[[Persona]]"
  - "[[Workspace]]"
  - "[[RuntimeSession]]"
---

# Plan - TF-0034 Authentication/Session Model

## Stabilized Planning Summary

TF-0034 introduces a minimal session model to support MVP workspace continuity. The boundary
is strict: session identity is not Persona, not canonical truth, not lifecycle authority, and
not a permission system.

The driving risk ADR 0022 addresses is conflating three distinct concepts that have different
semantic roles:

- **User identity** — who is operating the runtime.
- **Runtime session** — the current application continuity context.
- **Active workspace context** — the explicitly selected persona, persona version, workspace,
  workflow, and decision focus.

## Semantic Observations

- [[Persona]] remains a decision behavior model, not a user account.
- Persona activation must remain explicit inside workspace context and must not be inferred
  from user identity.
- Session context may provide workspace continuity defaults but must not own workspace truth.
- Replay must remain event-backed and must not depend on mutable browser session state.
- Historical decisions must preserve the persona context that shaped interpretation at the time,
  which requires session identity and persona to remain separately tracked.

## Architecture Boundaries

The implementation should preserve these boundaries:

- Session contracts are immutable app-layer value objects.
- The session provider is a pluggable protocol; `LocalSessionProvider` is sufficient for M7.
- `GET /session` is a read-only endpoint; it must not append events or mutate lifecycle state.
- Frontend may initialize workspace context from session defaults, but browser/session state
  is operational presentation state, not canonical truth.
- Full multi-user authorization, credential handling, and role-based policy remain out of scope.

## ADR Outcome

ADR 0022 (Authentication and Operational Identity) is required to define the identity/session
boundary before M8 workspace issues use session context. The ADR must be created before scaffold
files are merged.

## Scope

In scope for TF-0034:
- Immutable session contracts (`UserIdentity`, `SessionWorkspaceContext`, `RuntimeSession`).
- `SessionProvider` protocol and `LocalSessionProvider` implementation.
- `GET /session` API endpoint returning read-only session state with explicit authority labels.
- Frontend session consumption wired to workspace context initialization.
- ADR 0022 creation and issue register synchronization.

Out of scope:
- Full multi-user authorization model.
- Credential storage, OAuth, or token systems.
- Persona creation or activation from session.
- Session-backed canonical event writes.
