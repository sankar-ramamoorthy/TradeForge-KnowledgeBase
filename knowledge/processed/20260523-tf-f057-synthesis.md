---
title: TF-F057 Synthesis - LiteLLM Compose Runtime Service
type: processed-synthesis
status: processed
tags: [TradeForge, TF-F057, feedback, litellm, docker-compose, ai-advisory]
created: 2026-05-23
updated: 2026-05-23
source_history:
  - knowledge/raw/brainstorm-20260523-litellm-compose-runtime.md
  - knowledge/raw/20260523-tf-f057-diagnosis.md
  - knowledge/raw/20260523-tf-f057-planning.md
related:
  - "[[AI Advisory Boundary]]"
  - "[[Human Decision Sovereignty]]"
  - "[[Derived State Must Remain Distinguishable]]"
issues:
  - TF-F057
---

# TF-F057 Synthesis - LiteLLM Compose Runtime Service

## Feedback Identified

TradeForge advisory workflows expect an OpenAI-compatible LiteLLM endpoint, with local examples using `http://localhost:4000`. The local Docker Compose runtime did not include a LiteLLM service, so the advisory endpoint depended on an out-of-band process.

## Root Cause

The application advisory boundary was already implemented. The missing piece was operational composition: Docker Compose started TradeForge and Postgres but did not host the LiteLLM proxy.

## Change Made

- Added an optional `litellm` service to `docker-compose.yml` under the `advisory` profile.
- Exposed LiteLLM on host port `4000`.
- Added `litellm_config.yaml` with model aliases and environment-variable secret references.
- Updated README and credential setup docs with host-run versus container-run base URL guidance.
- Added focused tests for the Compose service and secret-free config.

## Invariants Involved

- AI remains advisory only.
- Human lifecycle authority is unchanged.
- LiteLLM output remains non-canonical advisory state.
- Provider API keys remain outside Git and outside runtime docs.

## Replay And Lifecycle Implications

No event, replay, or lifecycle behavior changed. The service only hosts an external advisory endpoint. Replay continues to depend on event history and replay-safe captured advisory facts, not live model calls.

## Verification

Completed:

- `uv run pytest tests\test_postgres_compose.py`
- `docker compose config`
- `docker compose --profile advisory config`
- `uv run pytest`
- `npm.cmd run typecheck`
- `npm.cmd run build`
- `docker compose --profile advisory up -d litellm`
- `docker compose --profile advisory ps litellm`
- `Invoke-WebRequest http://localhost:4000/health/readiness`

## Closure State

The runtime and documentation changes are implemented, regression-checked, and verified against the original observation. The optional LiteLLM container starts under the `advisory` profile and responds on `localhost:4000`.
