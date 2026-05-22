---
title: TF-F045 LiteLLM Credential Shape Synthesis
type: processed-synthesis
status: processed
created: 2026-05-22
updated: 2026-05-22
source_history:
  - knowledge/raw/archived/brainstorm-20260522-tf-f045-litellm-credential-shape.md
tags: [TradeForge, M13, TF-F045, LiteLLM, credential-boundary, AI-advisory]
related:
  - "[[20260522-m13-preparation-readiness-synthesis]]"
  - "[[20260522-llm-adapter-strategy-and-capability-review-synthesis]]"
  - "[[Provider Boundary Model]]"
---

# TF-F045 LiteLLM Credential Shape Synthesis

## Stabilized Outcome

TF-F045 is complete.

The runtime now has a typed LiteLLM credential payload shape for the M13 advisory
adapter path. The existing `CredentialStore` remains generic encrypted storage,
while the LiteLLM-specific helper owns the provider-specific payload shape:

```text
base_url
api_key
default_model
```

This preserves the M10C credential boundary while giving TF-F046 a clear
retrieval contract for the concrete OpenAI-compatible advisory provider.

## Architecture Result

The implementation extends the credential boundary without making the
CredentialStore provider-aware.

Stable implementation decisions:

- keep encrypted credential persistence generic
- model LiteLLM shape in a provider-specific helper
- provide a clear not-configured error for later adapter wiring
- extend the existing credential CLI with `register litellm`
- preserve the same encrypted `.keys.enc` workflow used by market-data providers

## Boundary Preservation

TF-F045 does not introduce advisory authority.

It does not:

- implement an LLM adapter
- call LiteLLM
- generate advisory content
- write advisory events
- change lifecycle authority
- store secrets in code or environment files

Credentials remain operational capability metadata under the existing
credential boundary.

## Follow-On Issue

The next M13 adapter issue is TF-F046: implement
`OpenAICompatibleAdvisoryProvider`.

TF-F046 should consume the LiteLLM credential shape and keep provider routing in
LiteLLM configuration rather than creating provider-specific TradeForge
adapters.

## Open Questions Deferred

The following remain future design concerns:

- whether advisory tasks need per-task model selection beyond `default_model`
- whether LiteLLM health checks should decrypt credentials directly or go
  through the adapter boundary
- whether the CLI should later gain a more specialized LiteLLM setup command

These questions do not block TF-F046.

