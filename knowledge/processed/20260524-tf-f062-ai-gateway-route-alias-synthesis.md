---
title: TF-F062 AI Gateway Route Alias Synthesis
type: processed-synthesis
status: processed
created: 2026-05-24
source_history:
  - knowledge/raw/archived/20260524-tf-f062-ai-gateway-route-alias-planning.md
  - knowledge/raw/archived/20260524-tf-f062-ai-gateway-route-alias-implementation.md
related_runtime:
  - DOCS/ai-gateway-route-alias-model.md
issue: TF-F062
milestone: M13A
tags:
  - ai-gateway
  - litellm
  - route-aliases
  - m13a
---

# TF-F062 AI Gateway Route Alias Synthesis

## Stabilized Knowledge

LiteLLM is an AI gateway and route boundary, not an ordinary single provider.
TradeForge should request semantic advisory roles or route aliases while the
gateway maps those roles to concrete underlying provider/model routes.

## Runtime Design Outcome

`DOCS/ai-gateway-route-alias-model.md` defines route aliases including
`tf-fast`, `tf-reasoning`, `tf-long-context`, `tf-cheap`, and `tf-local`, plus
representative mappings for replay summary, thesis review, advisory observation
generation, and candidate screening.

## Credential Boundary

TradeForge stores the gateway credential shape needed to call LiteLLM. Downstream
model-provider keys should live in LiteLLM configuration or operator environment
variables unless a future issue changes the credential boundary.

## Replay And Provenance

Advisory artifacts should preserve gateway provider, requested route alias,
underlying provider/model when available, timestamp, and advisory task kind.
Replay must not call live LiteLLM to reconstruct historical route resolution.

## Operational Sync

`TF-F062` is complete in the runtime issue register and marked complete in the
M13A roadmap issue list. TF-F063 is the next M13A issue.
