---
title: TF-F074 Governed LLM Provider Secret Management Synthesis
type: processed-synthesis
status: processed
created: 2026-05-25
source:
  - knowledge/raw/20260525-tf-f074-governed-llm-provider-secret-management.md
issue: TF-F074
milestone: M13B
tags:
  - tf-f074
  - m13b
  - llm-provider-secrets
  - credential-governance
  - litellm
---

# TF-F074 Governed LLM Provider Secret Management Synthesis

## Result

TF-F074 made TradeForge the governed owner of downstream LLM provider secrets
for the managed advisory runtime.

## Stabilized Runtime Pattern

Downstream LLM provider keys are stored as encrypted credentials:

```text
.keys.enc
  -> llm_groq
  -> llm_nvidia_nim
  -> llm_openai
  -> llm_anthropic
  -> llm_google
```

Each credential has an `api_key` payload field and follows the existing
CredentialStore, KeyManager, masking, validation, rotation, and reload pattern.

## Composition Boundary

Runtime decryption happens through:

```text
build_litellm_provider_environment()
```

This creates a composition-boundary environment projection for LiteLLM:

```text
GROQ_API_KEY
NVIDIA_NIM_API_KEY
OPENAI_API_KEY
ANTHROPIC_API_KEY
GOOGLE_API_KEY
```

Workflow code and advisory services do not decrypt provider secrets and do not
know how credentials are stored.

## Provider Governance Visibility

Provider Governance can now show downstream LLM provider secret status without
leaking secrets:

```text
GET /provider-governance/ai-gateway/provider-secret-injection
```

The endpoint exposes provider IDs, display names, target LiteLLM environment
variables, configured status, and composition-boundary injection status.

## Invariants Preserved

- Provider secrets are masked in UI/API responses.
- `TRADEFORGE_MASTER_KEY` remains OS environment configuration.
- AI remains advisory only.
- No lifecycle transitions are added.
- No event-ledger facts are written.
- No direct vendor SDK bypass is introduced.

## M13B Completion Meaning

With TF-F072, TF-F073, and TF-F074 complete, the M13B managed advisory runtime
slice has:

- TradeForge-mediated model discovery and route selection
- internal LiteLLM default network boundary
- governed downstream LLM provider secret storage
- composition-boundary secret projection

Future work may harden the managed LiteLLM launch/reload mechanism, but the
runtime governance boundary is now explicit and implemented in the local-first
credential model.
