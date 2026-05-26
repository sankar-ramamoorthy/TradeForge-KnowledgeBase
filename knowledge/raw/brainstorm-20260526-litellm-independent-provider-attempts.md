---
title: LiteLLM Independent Provider Attempts After NVIDIA NIM Validation
type: brainstorm
status: raw
tags: [tradeforge, brainstorm, m13b, tf-f075, litellm, nvidia-nim, provider-governance, diagnostics]
created: 2026-05-26
---

# LiteLLM Independent Provider Attempts After NVIDIA NIM Validation

## Trigger

During local runtime testing after Alembic was run, the advisory review path appeared to connect successfully through TradeForge and LiteLLM to the configured NVIDIA NIM endpoint. The advisory response looked coherent and returned a model tag consistent with NVIDIA NIM. However, runtime logs also showed failed attempts against other providers, including Gemini and Groq routes with invalid or missing API keys.

## Raw Input

The advisory review path appears to have successfully connected to the configured NVIDIA NIM endpoint.

Indicators included:

- Invocation state succeeded.
- A coherent generated advisory review was returned.
- The returned model tag included `nvidia nim/meta/llama-3.1-70b-instruct`.
- The output was structured inference output rather than a local fallback or error blob.

The current integration path is likely:

```text
TradeForge -> LiteLLM -> NVIDIA NIM -> response returned correctly
```

Other logged provider attempts may be independent of the NVIDIA NIM call rather than fallback-after-success behavior. Possible sources include:

- LiteLLM health checks.
- LiteLLM startup or route probing.
- Fallback provider resolution.
- Model availability checks.
- OpenAI-compatible endpoint probing.
- Previously configured fallback models or providers.
- Stale LiteLLM config.
- Stale container retry loops.
- Frontend polling or diagnostics after deleted backend/container state.
- Docker restart policy behavior.

The observed errors included fallback failures for Gemini and Groq routes, such as:

- `gemini/gemma-3-27b-it` with Google API key invalid / key None.
- `groq/openai/gpt-oss-20b` with invalid Groq API key.

The important point is that the advisory boundary appears operational, and the returned advisory output was aligned with TradeForge architecture:

- Non-canonical.
- Advisory-only.
- Caveated.
- Provenance attached.
- No lifecycle mutation.
- No autonomous approval.

This validates, at least partially:

- Provider credential resolution.
- LiteLLM routing.
- Request composition.
- Advisory invocation pipeline.
- UI rendering.
- Non-canonical cognition boundary.

The proposed first step is diagnosis only, not implementation. The diagnosis should determine whether the extra provider attempts are caused by TradeForge-controlled fallback, LiteLLM internals, frontend diagnostics, stale persisted selection, or stale containers.

## Observations

- NVIDIA NIM appears to be successfully reachable through the managed advisory route.
- The extra Gemini/Groq attempts may not be causally downstream of the successful NVIDIA response.
- The logs label some attempts as LiteLLM fallback failures, but that does not prove TradeForge initiated fallback after a successful primary call.
- Current architecture recently moved from model-string authority toward explicit provider ID plus model pairs, which is a likely area for stale or compatibility fallback behavior.
- LiteLLM wildcard route configuration may allow provider families that are not currently intended for active advisory use.
- Frontend Provider Governance and smoke-test flows may independently trigger gateway/model discovery or diagnostic calls.
- Docker health checks and restart behavior may produce log noise that looks like advisory retry behavior.

## Ideas

- Treat the immediate task as a diagnostic pass before any fix.
- Capture current governed model selection from TradeForge and verify whether fallback provider/model fields are null.
- Compare Provider Governance API state with the actual LiteLLM logs during one controlled advisory call.
- Trigger one explicit advisory smoke test and capture only the surrounding LiteLLM log window.
- Determine whether Gemini/Groq attempts occur:
  - on LiteLLM startup,
  - during `/health`,
  - during model listing,
  - during Provider Governance page load,
  - during smoke test,
  - during actual advisory generation,
  - or without any new TradeForge request.
- If a future fix is needed, preserve the desired behavior that successful NVIDIA NIM primary responses should not cause any fallback attempt.
- If fallback remains supported, constrain it to explicit TradeForge-governed provider/model pairs and only after primary failure.
- Consider pruning LiteLLM wildcard routes if diagnostics prove LiteLLM is attempting providers solely because wildcard routes are present.
- Consider adding provider-attempt provenance or diagnostics so operator-facing Provider Governance can explain attempted providers without making them canonical truth.

## Questions

- Are the Gemini/Groq attempts temporally correlated with the successful NVIDIA NIM advisory call, or are they independent startup/health/model-discovery behavior?
- Does the encrypted credential store still contain legacy `litellm.fallback_model` values?
- Does the active `advisory_model_selection` credential contain a fallback provider/model pair?
- Does LiteLLM perform background probes for wildcard models listed in `litellm_config.yaml`?
- Does the Provider Governance frontend fetch model-selection or smoke-test endpoints automatically in a way that causes model/provider calls?
- Are stale containers or restart loops still running after container deletion or recreation?
- Should the diagnostic output become a formal TF-F075 follow-up issue if it finds a code/config defect?

## Concerns

- Do not treat the successful NVIDIA NIM response as proof that all provider governance behavior is correct.
- Do not treat LiteLLM log wording alone as proof that TradeForge initiated the fallback.
- Avoid fixing this by adding provider keys for Gemini/Groq just to silence errors; that would hide the routing problem.
- Avoid reintroducing model-string authority after TF-F075 established explicit provider/model selection.
- Preserve the AI Advisory Boundary: provider diagnostics and route attempts are operational metadata, not canonical decision facts.
- Preserve Human Decision Sovereignty: no advisory output or diagnostic result may transition lifecycle state.
- Preserve Replayability: replay should not depend on live LiteLLM or provider probes.

## Possible Next Outputs

- Diagnostic checklist for the current runtime session.
- TF-F075 corrective issue candidate if hidden fallback or wildcard provider attempts are confirmed.
- Provider Governance diagnostics enhancement candidate.
- LiteLLM config tightening candidate.
- No action if attempts are proven to be stale container or one-time startup behavior.
