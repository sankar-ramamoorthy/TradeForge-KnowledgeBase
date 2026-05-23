---
title: TF-F046 OpenAI-Compatible Advisory Provider Synthesis
type: processed-synthesis
status: processed
created: 2026-05-22
updated: 2026-05-22
source_history:
  - knowledge/raw/archived/20260522-tf-f046-openai-compatible-advisory-provider-planning.md
  - knowledge/raw/archived/20260522-m13-completion-summary.md
tags: [TradeForge, M13, advisory, LLM, LiteLLM, provider-boundary]
related:
  - "[[20260522-llm-adapter-strategy-and-capability-review-synthesis]]"
  - "[[20260522-tf-f045-litellm-credential-shape-synthesis]]"
  - "[[Provider Boundary Model]]"
  - "[[AdvisoryArtifact]]"
---

# TF-F046 OpenAI-Compatible Advisory Provider Synthesis

## Stabilized Outcome

TF-F046 implemented a concrete OpenAI-compatible advisory provider for the
existing `AIAdvisoryProvider` protocol.

The provider consumes a decrypted LiteLLM credential payload from the
composition root and calls an OpenAI-compatible chat-completions endpoint. This
keeps provider routing in LiteLLM configuration rather than TradeForge domain
logic.

## Stable Design Decisions

- Use a single OpenAI-compatible provider instead of provider-specific Groq,
  NVIDIA NIM, Ollama, or Anthropic adapters.
- Read provider capability through the existing credential boundary.
- Keep decryption in the composition root and out of the adapter internals.
- Enforce per-artifact-kind prompt framing with an advisory safety footer.
- Return `AdvisoryResponse` objects with advisory authority only.
- Raise typed provider-unavailable errors for unreachable LLM infrastructure.

## Boundary Preservation

The provider does not own lifecycle authority, event authority, or semantic
truth. It is an infrastructure adapter behind a domain protocol.

Generated output must still pass advisory service validation and operator
acceptance before any durable advisory artifact is captured.

## Ontology Assessment

No new canonical entity is needed. The provider is runtime infrastructure.

The durable semantic concepts remain:

- [[AdvisoryArtifact]]
- [[AdvisoryObservation]]
- [[AdvisoryInterpretation]]
- [[AI Advisory Boundary]]

## Follow-On Implication

Future provider changes should remain LiteLLM configuration changes whenever
possible. A new provider-specific TradeForge adapter should require a concrete
constraint that the OpenAI-compatible path cannot satisfy.
