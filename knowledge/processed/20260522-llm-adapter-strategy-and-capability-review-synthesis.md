---
title: LLM Adapter Strategy And Capability Review Synthesis
type: processed-synthesis
status: processed
created: 2026-05-22
updated: 2026-05-22
source_history:
  - knowledge/raw/archived/20260522-llm-adapter-strategy-and-capability-review.md
  - knowledge/processed/20260522-m13-preparation-readiness-synthesis.md
authoritative_runtime_doc: C:/Users/bosto/dockerstuff/TradeForge/DOCS/llm-adapter-strategy.md
tags: [TradeForge, LLM, AI-advisory, provider-boundary, LiteLLM, M12, M13]
related:
  - "[[Machine-Assisted Discretionary Cognition]]"
  - "[[Provider Boundary Model]]"
  - "[[AdvisoryArtifact]]"
  - "[[AdvisoryObservation]]"
  - "[[AdvisoryInterpretation]]"
---

# LLM Adapter Strategy And Capability Review Synthesis

## Stabilized Outcome

The post-M12 capability review identified the concrete LLM adapter as the main
gap between advisory infrastructure and operational advisory intelligence.

M11 established the `AIAdvisoryProvider` protocol and advisory service
boundaries. M12 established durable advisory observation, candidate, artifact,
and replay-safe snapshot infrastructure. No concrete LLM provider adapter exists
yet, so advisory observations and candidates currently require manual entry or
research artifact import.

## Decision Preserved

The runtime strategy selected a single OpenAI-compatible advisory provider
pointed at LiteLLM.

This keeps TradeForge code provider-agnostic while LiteLLM handles concrete LLM
routing across Groq, NVIDIA NIM, Ollama, or future Claude API access.

The first implementation should not create provider-specific Groq, Anthropic, or
Ollama adapters unless a future constraint explicitly rejects LiteLLM.

## Sequencing Rule

M13 contextual interpretation should wait for a working concrete LLM adapter.

Building interpretation semantics before an upstream advisory intelligence
source exists would risk designing against hypothetical outputs rather than
validated advisory behavior.

The recommended sequence is:

```text
LiteLLM credential shape
    -> OpenAI-compatible advisory provider
    -> replay summary prompt
    -> thesis review prompt
    -> observation generation prompt
    -> candidate screening prompt
    -> on-demand operator trigger surfaces
    -> advisory provider health visibility
    -> future automatic enrichment hook specification
```

## Boundary Preservation

The adapter decision does not change:

- `AIAdvisoryProvider` protocol semantics
- `AIAdvisoryService` boundary enforcement
- advisory-only authority
- credential-store architecture
- lifecycle authority
- event taxonomy

LLM output remains advisory content. It must not write lifecycle events,
approve workflows, execute trades, or become canonical truth.

## Provider Capability Implication

LLM provider selection should follow the existing provider capability doctrine:
provider identity, provider capability, credentials, and fallback behavior must
remain explicit and auditable.

LiteLLM is a routing boundary, not a semantic authority. TradeForge still owns
the advisory contract, authority labels, prompt intent, and operator-facing
workflow integration.

## Operational Assessment

Local Ollama on the current machine is suitable only for lightweight structural
tasks, not substantive thesis review or multi-factor observation generation.

Groq through LiteLLM is the initial practical choice because it is already
tested locally and provides sufficiently strong free-tier model quality for
single-operator, on-demand advisory tasks.

NVIDIA NIM should be tested through LiteLLM before broader automation decisions.
Claude Pro consumer subscription does not provide Anthropic API access; Claude
API should be treated as a future LiteLLM-routed provider option if separate API
billing is added.

## Workflow Implications

The first delivery mode should be on-demand operator invocation.

Automatic enrichment on lifecycle events is a future workflow design problem.
Hook points should be specified before implementation, but background advisory
generation should not be built until prompt quality and operator value are
validated through explicit on-demand use.

## ADR Assessment

No new ADR is required for the adapter itself. Existing runtime ADRs governing
AI advisory boundaries, credential boundaries, and advisory observation
foundations are sufficient.

If future work lets LLM output mutate lifecycle state, write event-ledger facts
directly, or replace operator approval, that would violate current invariants
and require explicit rejection or a major architecture decision.

## Open Follow-Up

Runtime issue tracking has decomposed the strategy into TF-F045 through TF-F054.
The first implementation issue is TF-F045, followed by TF-F046 for the concrete
OpenAI-compatible advisory provider.

The authoritative ordered list lives in:

```text
C:/Users/bosto/dockerstuff/TradeForge/DOCS/llm-adapter-strategy.md
```
