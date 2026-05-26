---
title: TF-F076 LiteLLM Readiness Healthcheck Synthesis
type: synthesis
status: processed
tags: [tradeforge, tf-f076, m13b, litellm, healthcheck, advisory-runtime]
created: 2026-05-26
source:
  - knowledge/raw/brainstorm-20260526-litellm-independent-provider-attempts.md
  - knowledge/raw/logs from litellm container.txt
  - knowledge/raw/20260526-tf-f076-diagnosis.md
  - knowledge/raw/20260526-tf-f076-planning.md
---

# TF-F076 LiteLLM Readiness Healthcheck Synthesis

## Summary

TF-F076 resolves a managed advisory runtime hygiene gap: Docker readiness was calling LiteLLM `/health`, which performs provider/model health checks and can trigger real LLM API calls for configured routes. The fix changes the Compose healthcheck to `/health/readiness`, which checks proxy readiness without turning Docker health into provider route validation.

## Decision

Use `/health/readiness` for Docker Compose service health. Keep provider route validation explicit and TradeForge-mediated through governance smoke tests or intentional advisory requests.

## Runtime Boundary

LiteLLM remains internal-only infrastructure. The service keeps `LITELLM_MASTER_KEY` but receives no downstream provider API keys through Compose. Stateless request-time credential composition remains the accepted M13B boundary.

## Verification

Completed verification:

- `uv run pytest tests/test_postgres_compose.py` passed.
- `docker compose --profile advisory config` rendered the LiteLLM healthcheck against `/health/readiness`.
- `docker compose --profile advisory up -d` started the advisory profile and reported LiteLLM healthy.
- The idle LiteLLM log window showed repeated `GET /health/readiness` checks and no provider fallback errors.
- `POST /provider-governance/ai-gateway/smoke-test` returned `status: available` through TradeForge using `nvidia_nim/meta/llama-3.1-70b-instruct`.
- Post-smoke LiteLLM logs showed the explicit `/chat/completions` request and no Groq, Gemini, OpenAI, or Anthropic fallback attempts.

## Semantic Impact

No canonical semantics change. No lifecycle, event schema, credential schema, public API, domain model, or frontend behavior changed. The fix reinforces AI Advisory Boundary, Derived State Must Remain Distinguishable, Replayability Is Foundational, and Architectural Simplicity by keeping provider health probing separate from container readiness.
