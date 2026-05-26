
---

Here is a drop-in-ready raw brainstorm/planning note for your KB.

---

```markdown
---
title: M13A Provider Governance And AI Gateway Configuration
type: brainstorm
status: raw
tags:
  [
    tradeforge,
    provider-governance,
    ai-gateway,
    litellm,
    capability-routing,
    credentials,
    operational-ux,
    advisory-boundary,
    m13a,
  ]
created: 2026-05-23
---

# M13A — Provider Governance And AI Gateway Configuration

## Trigger

Current provider configuration exists only as a contextual rail embedded inside the Context Workbench.

The current implementation exposed several architectural and operational gaps:

- provider credentials are difficult to manage
- save/validation state is unclear
- provider capabilities are insufficiently modeled in the UI
- LiteLLM introduces multi-provider and multi-model routing complexity that does not fit the current one-provider-one-key mental model
- infrastructure administration is currently mixed with contextual cognition workflows
- provider governance concepts are becoming operationally important before deeper AI advisory work

M13 completed foundational provider capability architecture and contextual acquisition workflows.

M13A would formalize the operational governance layer around those systems.

---

# Core Realization

TradeForge is not merely configuring APIs.

TradeForge is establishing:

- external system governance
- provider trust boundaries
- capability routing
- AI gateway abstraction
- advisory provenance infrastructure
- operational diagnostics
- credential lifecycle management

This is an architectural subsystem, not a small UI enhancement.

---

# Architectural Direction

## Separation Of Concerns

Current implementation mixes:

- contextual cognition
- advisory acquisition
- provider administration
- credential management

inside a single contextual rail.

These concerns should be separated.

### Proposed Direction

Contextual rails should become:

- informational
- provenance-oriented
- operationally contextual

while provider administration moves into a dedicated workspace subsystem.

---

# Proposed Workspace

## Provider Configuration Workspace

Example navigation structure:

- Overview
- Credentials
- Market Data Providers
- Broker Providers
- AI Gateway
- Capability Routing
- Diagnostics

This becomes the operational control plane for external systems.

---

# Key Architectural Insight

TradeForge should think in:

- capabilities
- operational roles
- advisory intent

rather than raw vendors or model names.

Examples:

- price snapshot provider
- fundamentals provider
- paper trading provider
- AI advisory provider
- replay enrichment provider

rather than:

- FMP
- Alpaca
- OpenAI
- Anthropic

---

# Capability Routing Concept

Provider selection should occur per capability.

Example:

- Price Snapshots
  - primary: yfinance
  - fallback: polygon

- Fundamentals
  - primary: fmp
  - fallback: alpha_vantage

- Paper Trading
  - primary: alpaca

- AI Advisory
  - primary: litellm

This preserves provider abstraction boundaries.

---

# LiteLLM Architectural Realization

LiteLLM is not merely another provider.

It is effectively:

- AI gateway
- model router
- abstraction layer
- policy boundary
- future orchestration boundary

TradeForge should not hardcode raw model names throughout workflows.

Instead TradeForge should operate using semantic AI roles.

Example:

- thesis critique
- replay analysis
- fast summarization
- evidence extraction
- plan review assistant

LiteLLM routes would map those semantic roles to concrete underlying providers/models.

---

# Proposed LiteLLM Model Routing Concept

Example:

| Role | Route Alias | Underlying Provider |
|---|---|---|
| Fast Summary | tf-fast | Groq |
| Thesis Critique | tf-reasoning | OpenAI or Anthropic |
| Replay Analysis | tf-long-context | Claude |
| Cheap Classification | tf-cheap | Gemini |
| Offline Local | tf-local | Ollama |

TradeForge requests:

- "thesis critique"

rather than:

- "gpt-4.1"

This preserves future provider flexibility.

---

# Credential Management Problems Observed

Current implementation problems:

- save feedback unclear
- no visible validation state
- no health status
- no testing workflow
- unclear error handling
- difficult to distinguish configured vs invalid credentials
- no operational diagnostics
- no rotation workflow

---

# Proposed Credential Management Model

Per provider:

- configured state
- validation state
- health status
- last validation timestamp
- rotation workflow
- test connection workflow
- remove credential workflow

Credentials should not remain permanently visible after save.

---

# Proposed Provider Overview UX

The overview page should communicate operational state quickly.

Example sections:

- Provider Health
- Capability Routing
- Credential Status
- AI Gateway Status
- Diagnostics / Recent Failures
- Advisory Boundary Reminder

The UI should feel:

- operational
- infrastructure-oriented
- deterministic
- trustworthy

rather than consumer SaaS configuration UX.

---

# Contextual Rail Future Direction

The contextual rail should eventually contain:

- provenance
- provider source attribution
- freshness
- fallback usage
- uncertainty
- replay integrity
- acquisition diagnostics
- advisory warnings

rather than direct credential management forms.

---

# Diagnostics Importance

Future AI and provider orchestration will require strong diagnostics.

Potential examples:

- provider unreachable
- fallback triggered
- quota exceeded
- LiteLLM route unavailable
- invalid credential
- provider latency spike
- replay nondeterminism warning

Operational trust depends on visibility.

---

# Architectural Relationship To Future Milestones

M13A appears foundational for:

- future AI advisory subsystem work
- replay-aware advisory orchestration
- multi-model AI routing
- provider fallback governance
- paper trading provider integration
- future execution safety boundaries
- external system observability

This milestone likely reduces future architectural drift before M14+ AI work expands complexity.

---

# Possible Scope

## In Scope

- dedicated Provider Configuration workspace
- provider overview/status pages
- capability routing UI
- credential management workflows
- LiteLLM gateway configuration
- model role routing concepts
- diagnostics/event visibility
- operational/provider health surfaces
- contextual rail simplification

## Explicitly Out Of Scope

- autonomous trading
- AI decision authority
- AI plan approval
- automated execution
- broker automation expansion
- full orchestration engine
- AI memory/cognition systems

---

# Open Questions

- should provider configuration events become replay-visible artifacts
- how much provider history should be retained
- should provider health become event sourced
- should AI route selection become operator-configurable per workflow
- should TradeForge support provider weighting policies
- how should local/offline models be represented operationally
- should replay sessions record underlying AI route aliases
- should model cost visibility exist
- how deterministic should AI route replay become
- should capability routing become workspace-aware
```


Yes — this fits very naturally into the roadmap structure you already have. 

And after reviewing the roadmap progression, I think positioning this as:

# M13A — Provider Governance And AI Gateway Configuration

is architecturally coherent.

Why?

Because M13 completed:

* contextual interpretation
* advisory cognition
* LiteLLM integration foundations
* UI credential management
* advisory provider abstraction

But what you discovered operationally is:

> the governance/control-plane layer around providers is still immature.

That is exactly the kind of “stabilization and formalization” mini-milestone that belongs immediately after M13 and before M14 behavioral intelligence expansion. 

The roadmap already shows the evolution:

```text
M10C -> credential boundary
M10D -> capability-aware provider architecture
M10E -> context acquisition workflows
M11  -> advisory AI boundary
M12  -> advisory observations
M13  -> contextual interpretation
```

M13A becomes:

```text
operational governance and orchestration layer
for the now-expanded provider + AI ecosystem
```

Which is actually a very logical continuation.

A few especially important realizations from the roadmap:

* M10C established credential security but not operational governance
* M10D established provider capability semantics but not sophisticated routing UX
* M13 added LiteLLM and advisory providers but still treated configuration too simplistically
* The current Context Workbench rail accidentally mixes:

  * cognition
  * provider administration
  * infrastructure management
  * AI routing

That mixing is exactly the architectural smell M13A should resolve.

I also think this line from your roadmap is especially important context for M13A:

> “TradeForge should think in capabilities and advisory intent rather than raw vendors.” (implicit across M10D + M13) 

Because LiteLLM changes the game:

* one “provider” can represent many underlying providers
* semantic roles become more important than raw model names
* routing/orchestration becomes operationally meaningful
* diagnostics and provenance become essential

So M13A is not “settings UI.”

It is really:

```text
External Systems Governance
And AI Routing Infrastructure
```

which is a substantial architectural concern.

I think your raw note is already directionally excellent. What I would do next:

# Recommended Next KB Artifacts

After this brainstorm/raw note, eventually promote toward:

* `design/provider-configuration-workspace.md`
* `design/provider-routing-model.md`
* `design/litellm-gateway-architecture.md`
* `design/provider-diagnostics-and-health.md`
* `ontology/provider-capability.md`
* `ontology/ai-route-alias.md`
* `adr/ADR-00XX-provider-governance-boundary.md`

Because this is now crossing from:

* implementation detail

into:

* stable operational doctrine.
