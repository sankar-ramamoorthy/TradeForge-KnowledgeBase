---
title: TF-F073 Internalize LiteLLM Gateway Network Boundary Synthesis
type: processed-synthesis
status: processed
created: 2026-05-25
source:
  - knowledge/raw/20260525-tf-f073-internalize-litellm-gateway-boundary.md
issue: TF-F073
milestone: M13B
tags:
  - tf-f073
  - m13b
  - litellm
  - docker
  - advisory-runtime
---

# TF-F073 Internalize LiteLLM Gateway Network Boundary Synthesis

## Result

TF-F073 changed LiteLLM from a host-exposed local service into Docker-internal
managed advisory infrastructure by default.

## Stabilized Runtime Pattern

Default runtime path:

```text
Browser
  -> TradeForge HTTP API
  -> TradeForge advisory provider boundary
  -> http://litellm:4000 on Docker internal network
  -> LiteLLM model routes
```

Direct host access to `localhost:4000` is no longer part of the default managed
runtime.

## Debug Pattern

Temporary host access is still possible through an explicit override:

```text
docker compose --profile advisory -f docker-compose.yml -f docker-compose.litellm-debug.yml up -d litellm
```

This keeps direct LiteLLM inspection intentional instead of part of normal
operator flow.

## Invariants Preserved

- TradeForge remains the operator-facing advisory gateway.
- LiteLLM remains advisory infrastructure and does not own lifecycle authority.
- Provider Governance and route smoke tests remain TradeForge-mediated.
- No event-ledger facts are written by gateway exposure changes.
- No lifecycle state or decision truth changed.

## Deferred Work

TF-F074 still owns downstream LLM provider secret governance and managed
injection into LiteLLM.

Production platform networking, Kubernetes, external vaults, and distributed
inference remain out of scope.
