---
title: Provider Governance And AI Gateway Configuration Brainstorm
type: brainstorm
status: raw
tags: [tradeforge, brainstorm, provider-governance, ai-gateway, litellm, capability-routing, credentials, operational-ux, m13a]
created: 2026-05-23
---

# Provider Governance And AI Gateway Configuration Brainstorm

## Trigger

Provider configuration and credential entry currently feel incorrectly placed inside contextual workflow surfaces. Recent ChatGPT discussion captured across three raw files identified that this is not just a UI bug or missing save feedback. It may indicate a larger architectural gap around provider governance, credential lifecycle management, capability routing, LiteLLM model routing, and operational diagnostics.

Source raw files:

- `knowledge/raw/Brainstorm Notes  about Dedicated Provider configuration v1.md`
- `knowledge/raw/Brainstorm Notes  about Dedicated Provider configuration v2.md`
- `knowledge/raw/Brainstorm Notes  about Dedicated Provider configuration session 3.md`

## Raw Input

The source conversation argued that credential UX/design should be fixed before provider configuration hardens into accidental architecture.

Important raw statements and fragments:

- "Credential != Provider != Capability != Model"
- "LiteLLM is not one provider. LiteLLM is a gateway."
- TradeForge should choose roles, not raw model names everywhere.
- The right rail should only show provider status and provenance; clicking configure should lead to a full Provider Configuration workspace.
- Provider configuration is becoming an operational control plane, provider governance layer, and AI routing boundary.
- This should not feel like a generic SaaS admin panel; it should feel operational, explicit, deterministic, and infrastructure-grade.
- TradeForge should think in capabilities, operational roles, and advisory intent rather than raw vendors.
- The discovered issue spans provider governance, capability routing, credential lifecycle management, AI gateway abstraction, operational diagnostics, and external-system trust boundaries.
- The proposed milestone name was `M13A - Provider Governance And AI Gateway Configuration`.
- The proposed framing was: a post-M13 operational infrastructure milestone that formalizes external provider governance, credential management, capability routing, and AI gateway abstraction before deeper AI advisory integration.

Representative proposed workspace structure:

```text
Provider Configuration

- Overview
- Credentials
- Market Data Providers
- Broker Providers
- AI Gateway
- Capability Routing
- Diagnostics
```

Representative capability routing model:

```text
Price Snapshots
  Primary: yfinance
  Fallback: polygon

Fundamentals
  Primary: fmp
  Fallback: alpha_vantage

Paper Trading
  Primary: alpaca

AI Advisory
  Primary: litellm
```

Representative LiteLLM role routing model:

```text
Fast Summary          -> tf-fast          -> Groq
Thesis Critique       -> tf-reasoning     -> OpenAI or Anthropic
Replay Analysis       -> tf-long-context  -> Claude
Cheap Classification  -> tf-cheap         -> Gemini
Offline Local         -> tf-local         -> Ollama
```

The raw conversation emphasized that TradeForge should request semantic AI roles such as "thesis critique" or "replay analysis" rather than hardcoding raw model names like `gpt-4.1` throughout workflows.

## Observations

- Provider configuration is currently too tightly coupled to contextual cognition surfaces.
- Credential management, provider selection, capability routing, and model routing are separate concepts but are easy to collapse into one UI and one mental model.
- LiteLLM changes the provider model because one gateway can route to many underlying model providers.
- Provider and AI gateway diagnostics will become more important as advisory systems expand.
- The current right-rail approach risks mixing cognition, provenance, administration, and infrastructure management in one cramped surface.
- Provider data remains non-canonical and advisory; this boundary must stay visible in future UX.
- This appears larger than a feedback bug and may justify a milestone-level stabilization pass.

## Ideas

- Create a dedicated Provider Configuration workspace or workspace subsystem.
- Keep contextual rails informational: provider source, selected capability, freshness, fallback status, health, provenance, advisory boundary.
- Move credential entry, rotation, deletion, validation, and testing into the dedicated provider workspace.
- Model provider selection by capability rather than by raw vendor.
- Treat LiteLLM as an AI gateway and routing boundary rather than as a normal data provider.
- Use semantic AI role aliases such as `thesis critique`, `fast summary`, `replay analysis`, and `evidence extraction`.
- Make route aliases map to underlying providers and models outside core workflow logic.
- Add operational diagnostics such as invalid credential, quota exceeded, fallback triggered, latency spike, route unavailable, provider unreachable, and replay nondeterminism warning.
- Consider milestone `M13A - Provider Governance And AI Gateway Configuration` as a post-M13 stabilization milestone before broader AI advisory expansion.

## Questions

- Should provider configuration events become replay-visible artifacts?
- How much provider configuration and health history should be retained?
- Should provider health be event-sourced, logged operationally, or treated as ephemeral diagnostics?
- Should AI route selection be operator-configurable per workflow?
- Should TradeForge support provider weighting policies or only primary/fallback routing?
- How should local/offline models be represented in provider governance?
- Should replay sessions record the route alias only, or also the resolved underlying provider/model at the time?
- Should model cost visibility become part of the provider workspace?
- Should capability routing become workspace-aware or remain globally configured?

## Concerns

- Treating this as a small settings page may hide an architectural boundary that needs explicit design.
- Hardcoding raw AI models into workflow logic would weaken provider flexibility and replay interpretation.
- Credential UX without validation, confirmation, or diagnostics can create false operator confidence.
- Provider administration inside contextual rails may fragment cognition and obscure provenance.
- Overbuilding orchestration too early could introduce unnecessary complexity before the operational model stabilizes.
- Any future provider governance must preserve human decision sovereignty and keep AI advisory, never authoritative.
- External provider data must remain distinguishable from canonical event-ledger truth.

## Possible Next Outputs

- Issue candidate
- M13A milestone candidate
- Provider Configuration workspace design note
- Provider routing model design note
- LiteLLM gateway architecture design note
- Provider diagnostics and health design note
- Provider capability ontology candidate
- AI route alias ontology candidate
- ADR candidate for provider governance boundary
