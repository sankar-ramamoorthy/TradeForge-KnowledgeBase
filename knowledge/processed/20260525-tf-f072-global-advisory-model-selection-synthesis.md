---
title: TF-F072 Global Advisory Model Selection Synthesis
type: processed-synthesis
status: processed
created: 2026-05-25
source:
  - knowledge/raw/20260525-tf-f072-global-advisory-model-selection.md
issue: TF-F072
milestone: M13B
tags:
  - tf-f072
  - m13b
  - ai-gateway
  - model-selection
  - provider-governance
---

# TF-F072 Global Advisory Model Selection Synthesis

## Result

TF-F072 was implemented as a governed global advisory route selection layer.

The selected model remains operational configuration. It is not canonical
decision truth, does not create lifecycle authority, and does not add event
types.

## Stabilized Runtime Pattern

TradeForge uses the existing LiteLLM credential as the advisory model
selection boundary:

```text
litellm credential
  -> base_url
  -> api_key
  -> default_model
  -> optional fallback_model
```

The `default_model` field is the primary advisory route. The optional
`fallback_model` is used only when the primary route is unavailable.

This keeps advisory workflow code independent from raw model names. Workflow
services still call the `AIAdvisoryProvider` port and do not select models
directly.

## TradeForge-Mediated Discovery

Model discovery now happens through TradeForge:

- `GET /provider-governance/ai-gateway/model-selection`
- provider adapter calls LiteLLM `/v1/models` through the OpenAI-compatible
  client
- API returns available models, selected primary model, selected fallback model,
  gateway URL, and operational authority flags

## TradeForge-Mediated Selection

Model selection updates now happen through TradeForge:

- `PUT /provider-governance/ai-gateway/model-selection`
- updates the encrypted LiteLLM credential payload in `.keys.enc`
- reloads the in-process advisory provider
- does not append events
- rejects unavailable models when model discovery succeeds

## Frontend Surface

Provider Governance now includes model-selection controls inside the AI Gateway
section. Operators can choose a primary route and optional fallback from the
discovered model list and save the selection before running smoke tests.

## Invariants Preserved

- AI remains advisory only.
- Human decision sovereignty remains unchanged.
- No lifecycle transition behavior changed.
- No event-ledger facts are written by model discovery, selection, or smoke
  testing.
- Replay should rely on captured advisory provenance, not current live route
  discovery.

## Deferred Work

TF-F073 still owns internalizing the LiteLLM network boundary.

TF-F074 still owns downstream LLM provider secret governance and managed
runtime injection.

Per-task dynamic routing, hidden model choice, orchestration, and cost
optimization remain out of scope.
