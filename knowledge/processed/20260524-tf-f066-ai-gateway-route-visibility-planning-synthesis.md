---
title: TF-F066 AI Gateway Route Visibility Planning Synthesis
type: processed-synthesis
status: processed
created: 2026-05-24
source_raw:
  - knowledge/raw/20260524-tf-f066-ai-gateway-route-visibility-planning.md
source_issue: TF-F066
milestone: M13A
tags:
  - TradeForge
  - M13A
  - ai-gateway
---

# TF-F066 AI Gateway Route Visibility Planning Synthesis

TF-F066 deepens LiteLLM visibility inside the provider governance boundary. The work should expose gateway and route metadata without turning AI routing into workflow semantics or lifecycle authority.

## Stabilized Design

AI gateway visibility should show:

- LiteLLM gateway identity
- gateway URL when safely decryptable
- default model or route target when safely decryptable
- inferred underlying provider when the model identifier includes a provider prefix
- TradeForge route aliases and advisory usage domains
- configured/unconfigured route availability
- reachability as non-generative operational status

## Boundary Preservation

The read path must not expose API keys. Route aliases remain TradeForge-facing advisory roles, not canonical facts and not hard dependencies inside workflow logic.

## ADR Result

No new ADR is required; this implements existing AI gateway and advisory-boundary decisions.
