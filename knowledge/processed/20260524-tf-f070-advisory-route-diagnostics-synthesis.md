---
title: TF-F070 Synthesis - Advisory Route Diagnostics And Explicit Advisory Invocation
type: processed-synthesis
status: processed
tags:
  - TradeForge
  - TF-F070
  - feedback
  - advisory
  - litellm
  - provider-governance
created: 2026-05-24
updated: 2026-05-24
source_history:
  - knowledge/raw/brainstorm-20260524-litellm-advisory-routing-test-gap.md
  - knowledge/raw/TF-F069 Provider governance page not working.md
  - knowledge/raw/20260524-tf-f070-diagnosis.md
  - knowledge/raw/20260524-tf-f070-planning.md
  - knowledge/raw/Implemented TF-F070 and ran the feedback loop.md
archived_sources:
  - knowledge/raw/archived/Implemented TF-F070 and ran the feedback loop.md
related:
  - "[[AI Advisory Boundary]]"
  - "[[UX Is Architectural]]"
  - "[[Derived State Must Remain Distinguishable]]"
issues:
  - TF-F070
---

# TF-F070 Synthesis - Advisory Route Diagnostics And Explicit Advisory Invocation

## Feedback Signal

The operator could verify local Ollama directly and could verify LiteLLM manually, but the TradeForge workflow did not make it clear whether an advisory generation request had actually been invoked from Plan Review. Groq showed no activity for the day, and the Plan Review advisory panel exposed a raw `JSON.parse: unexpected character at line 1 column 1` failure, which made provider failure, LiteLLM routing failure, backend route failure, and Vite proxy failure indistinguishable.

## Diagnosis

The runtime architecture intends AI advisory behavior to be explicit, non-canonical, and operator-invoked. The observed Plan Review flow did not show evidence of an advisory generation request. The local Vite proxy also lacked advisory route coverage, so `/advisory/*` requests could be served the frontend HTML shell instead of FastAPI JSON. That produced a misleading JSON parse error before provider behavior could be meaningfully evaluated.

The problem was therefore not treated as autonomous advisory generation failure. It was treated as an operational diagnostics and invocation visibility gap.

## Implementation Outcome

TF-F070 added local development proxy coverage for `/advisory`, `/provider-governance`, and `/admin`; explicit operational JSON parsing for advisory-related frontend calls; an operator-triggered `Generate Advisory Review` action in Plan Review; and a Provider Governance AI gateway smoke test at `POST /provider-governance/ai-gateway/smoke-test`.

The smoke test calls the configured advisory provider only when the operator asks for it, returns operational metadata only, and does not create canonical event-ledger facts. Provider credential reload now also refreshes the in-process LiteLLM advisory provider so saved LiteLLM settings can become active without a backend restart.

## Semantic Boundary

The implementation preserves the AI Advisory Boundary: advisory output can interpret and summarize but cannot approve, execute, transition lifecycle state, or become canonical truth by itself. It preserves Human Decision Sovereignty by making invocation explicit. It preserves replayability by avoiding event writes for diagnostics and by keeping smoke-test results out of canonical ledger history.

## Verification

Focused backend tests confirmed provider-governance smoke tests use the advisory provider without event writes and report not-configured state without event writes. Frontend typecheck and build passed. Backend typing passed for the touched API route and tests. `git diff --check` passed.

Runtime verification recorded by the implementation capture:

```text
uv run pytest tests/test_provider_governance_api.py tests/test_default_advisory_provider_bootstrap.py
uv run mypy src\app\api\routes.py tests\test_provider_governance_api.py
uv run ruff check tests\test_provider_governance_api.py
npm.cmd run typecheck
npm.cmd run build
git diff --check
```

`ruff` still reports pre-existing line-length and FastAPI `Query(...)` default findings in `src/app/api/routes.py`; the TF-F070-added long line was fixed and the remaining findings were left outside scope.

## Processing Findings

- Semantic implication: TF-F070 stabilizes explicit operator invocation and operational diagnostics for advisory generation, without redefining advisory authority.
- Ontology implication: no new canonical entity is required; existing `Advisory Artifact`, `Advisory Candidate`, `Decision Lifecycle Engine`, and `Event Ledger` boundaries remain sufficient.
- Workflow implication: the feedback loop closed after implementation verification; the runtime issue can be treated as processed from a KB perspective.
- ADR implication: no ADR is required because the issue reinforces existing AI advisory and provider governance boundaries rather than introducing a new architectural decision.
- Operational synchronization note: full `ruff` on `src/app/api/routes.py` remains blocked by pre-existing findings outside TF-F070 scope; this should remain backlog context, not TF-F070 completion debt.
