---
title: TF-F070 Diagnosis - Advisory Route Diagnostics And Explicit Advisory Invocation
type: feedback-diagnosis
status: raw
tags: [TradeForge, TF-F070, feedback, advisory, litellm, provider-governance, frontend]
created: 2026-05-24
issues: [TF-F070]
source_history:
  - knowledge/raw/brainstorm-20260524-litellm-advisory-routing-test-gap.md
  - knowledge/raw/TF-F069 Provider governance page not working.md
---

# TF-F070 Diagnosis - Advisory Route Diagnostics And Explicit Advisory Invocation

## Observable Gap

Operators can configure LiteLLM and can manually prove LiteLLM routes to Ollama, but the TradeForge UI does not clearly show whether advisory generation was invoked, whether the gateway route was reachable, or whether a failure came from local dev proxying, the API, LiteLLM, or the underlying provider.

The Plan Review Workspace displayed a raw JSON parse failure in the advisory interpretations panel after thesis creation. That made the failure look like an LLM/provider problem even though logs showed no advisory generation call occurred.

## Root Cause

The gap has two implementation causes:

- `frontend/vite.config.ts` proxies `/provider-governance` and `/admin`, but not `/advisory`. Local Vite development can therefore return the React HTML shell for advisory API calls, and frontend code then attempts to parse HTML as JSON.
- Plan Review currently renders advisory interpretation history but has no explicit operator action to invoke the existing `/advisory/thesis-review` generation endpoint. Entering a thesis and navigating to Plan Review correctly does not auto-call the LLM, but the UI does not make that distinction obvious.

## Existing Working Boundaries

- LiteLLM can expose route aliases such as `tradeforge-ollama`.
- The OpenAI-compatible provider and `/advisory/thesis-review` endpoint already exist.
- Provider governance already exposes gateway metadata without secrets.
- Advisory generation responses are non-canonical drafts requiring operator acceptance.

## Relevant Invariants

- AI Advisory Boundary: generated content is advisory and cannot approve, execute, or transition lifecycle state.
- Human Decision Sovereignty: all material workflow decisions remain operator-owned.
- Derived State Must Remain Distinguishable: diagnostics and advisory state must not become canonical facts.
- Replayability Is Foundational: route smoke tests must not write canonical ledger events.
- UX Is Architectural: failures must show operational meaning rather than raw parse errors.

## Minimum Correct Scope

- Fix the local dev proxy for `/advisory`.
- Improve advisory fetch error handling for non-JSON responses.
- Add explicit operator-invoked thesis advisory review in Plan Review.
- Add a provider governance smoke test that calls the configured advisory route with a small diagnostic prompt and reports operational result only.

## Explicit Non-Scope

- No automatic advisory generation during lifecycle transitions.
- No new lifecycle events or stages.
- No hidden background LLM calls.
- No persistent AI memory.
- No orchestration engine or multi-agent workflow.
- No editing external LiteLLM config from TradeForge.
