---
title: TF-F072 Global Advisory Model Selection Implementation Note
type: implementation-note
status: raw
created: 2026-05-25
issue: TF-F072
milestone: M13B
tags:
  - tf-f072
  - m13b
  - ai-gateway
  - model-selection
  - provider-governance
---

# TF-F072 Global Advisory Model Selection Implementation Note

## Context

TF-F072 implements the first M13B issue after the M13B planning promotion.
The goal is global advisory model selection through TradeForge:

- discover LiteLLM models/routes through TradeForge
- select a primary advisory model/route
- select an optional fallback model/route
- remove workflow dependence on hardcoded advisory model names
- apply the selected configuration advisory-wide
- smoke-test the selected route through TradeForge

## Implementation Summary

The existing `litellm` credential remains the governed configuration boundary.
It now supports optional `fallback_model` while preserving backward
compatibility with existing credentials that only have `default_model`.

The OpenAI-compatible advisory provider now:

- uses `default_model` as the selected primary route
- tries `fallback_model` only when the primary route is unavailable
- exposes `list_models()` for TradeForge-mediated LiteLLM model discovery

Provider Governance now exposes:

- `GET /provider-governance/ai-gateway/model-selection`
- `PUT /provider-governance/ai-gateway/model-selection`

The update endpoint modifies only the encrypted LiteLLM credential payload,
reloads the in-process advisory provider, and does not write canonical events.

The frontend Provider Governance surface now shows available models, selected
primary route, selected fallback route, and save controls.

## Boundary Notes

No lifecycle behavior changed.

No event types were added.

No Docker networking changed.

No downstream vendor secret ownership was added; TF-F074 owns that.

No per-task dynamic routing or orchestration was added.

## Verification

- `uv run pytest tests/test_credential_store.py tests/test_admin_credentials.py tests/test_openai_compatible_provider.py tests/test_provider_governance_api.py tests/test_default_advisory_provider_bootstrap.py`
- `uv run mypy src\security\litellm_credential.py src\infrastructure\advisory\openai_compatible_provider.py src\app\api\routes.py src\app\api\admin_routes.py tests\test_credential_store.py tests\test_openai_compatible_provider.py tests\test_provider_governance_api.py`
- `uv run ruff check src\security\litellm_credential.py src\infrastructure\advisory\openai_compatible_provider.py tests\test_credential_store.py tests\test_openai_compatible_provider.py tests\test_provider_governance_api.py scripts\manage_credentials.py`
- `npm.cmd run typecheck`
- `npm.cmd run build`
