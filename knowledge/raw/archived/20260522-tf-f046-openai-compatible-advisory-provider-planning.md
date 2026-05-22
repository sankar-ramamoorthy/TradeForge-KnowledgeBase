# TF-F046: OpenAICompatibleAdvisoryProvider — Planning Note

## Date
2026-05-22

## Issue
TF-F046 — Implement OpenAICompatibleAdvisoryProvider

## Problem
`AIAdvisoryProvider` protocol exists with no concrete implementation.
`InterpretationDraftService`, `ReplayAdvisoryService`, and `ReviewAdvisoryService`
cannot generate advisory content. All four M13 advisory tasks are blocked.

## Approach

### New file: `src/infrastructure/advisory/openai_compatible_provider.py`

Class: `OpenAICompatibleAdvisoryProvider`
Implements: `AIAdvisoryProvider` protocol

Constructor: receives `LiteLLMCredentialPayload` (already decrypted — the
composition root is responsible for decryption, not the adapter).

`provider_id` property: returns `"litellm"` (LITELLM_PROVIDER_ID).
`provider_version` property: returns `"openai-compatible-v1"`.

`generate(request: AdvisoryRequest) -> AdvisoryResponse`:
1. Build system prompt from a per-artifact-kind template.
2. Build user message from `request.operator_question` + `request.context_summary`.
3. POST to `{base_url}/v1/chat/completions` using `openai` client or `httpx`.
4. Parse response content.
5. Build and return `AdvisoryResponse` with:
   - `request_id` matching the request
   - `artifact_kind` matching the request
   - `content` from the LLM response
   - `provenance` with provider_id, model_id, generated_at, prompt_version
   - `uncertainty` with conservative confidence (0.5) and standard caveats
   - `source_references` echoing the request source_references
   - `authority = AdvisoryAuthority.ADVISORY`

### System prompt strategy (per artifact kind)
All prompts share a shared safety footer:
```
You are an advisory system. Your output is non-canonical advisory context.
Do NOT issue buy/sell instructions, lifecycle transition recommendations,
trade approval language, or execution authority. Always express uncertainty.
Always cite your reasoning. Always include caveats.
```

Kind-specific prefixes:
- REPLAY_SUMMARY: "Summarize the following trading decision replay..."
- INTERPRETATION_DRAFT: "Interpret the following advisory observations..."
- REVIEW_ASSISTANCE: "Assist with reviewing the following trade..."
- THESIS_REVIEW: "Review the following trade thesis for blind spots..."
- OBSERVATION_GENERATION: "Generate advisory observations for..."
- CANDIDATE_SCREENING: "Screen the following advisory candidates..."

### HTTP client choice
Use the `openai` Python package — it already supports custom `base_url`,
which is how LiteLLM/Groq/NVIDIA NIM are accessed. Avoids raw httpx and
handles retry, timeout, streaming off by default.

Dependency: `openai>=1.0` — add to pyproject.toml.

### Composition root wiring (`src/app/api/application.py`)
Add after credential store load:
```python
advisory_provider = _build_advisory_provider(credential_store, key_manager)
```
`_build_advisory_provider()`: calls `get_litellm_credential()`, constructs
`OpenAICompatibleAdvisoryProvider`. If credential not configured, returns
`None` (advisory is optional — lifecycle/market systems continue to work).

Inject `advisory_provider` into `AIAdvisoryService` and
`InterpretationDraftService` where needed.

### Error handling
- `LiteLLMCredentialNotConfiguredError` → advisory unavailable, not a fatal
  app startup error
- HTTP errors from LiteLLM → raise `AdvisoryProviderUnavailableError`
  (new exception in domain)
- Response validation failures → raise `ValueError` (advisory boundary
  contract violation)

## ADR impact
ADR-0006 governs — no new ADR needed. This is the implementation of the
already-accepted boundary.

## Testing strategy
- Unit tests with a mock HTTP server or monkeypatched `openai.OpenAI` client
- Test that `authority=ADVISORY` is enforced on response
- Test that mismatched `request_id` or `artifact_kind` raises
- Test `AdvisoryProviderUnavailableError` on HTTP failure
- Integration-style: test with a stub `openai.OpenAI` that returns a valid
  completion response

## Files to create/modify
- CREATE: `src/infrastructure/advisory/openai_compatible_provider.py`
- MODIFY: `src/app/api/application.py` (wire provider)
- MODIFY: `src/infrastructure/advisory/__init__.py` (export)
- MODIFY: `pyproject.toml` (add openai dependency)
- CREATE: `tests/test_openai_compatible_provider.py`
