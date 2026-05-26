---
title: M13B AI Gateway Governance And Managed Advisory Runtime
type: brainstorm
status: raw
tags: [trading-system, brainstorm, m13b, ai-gateway-governance, advisory-runtime, litellm, credential-governance]
created: 2026-05-25
---

# M13B AI Gateway Governance And Managed Advisory Runtime

## Trigger

M13A established the first LiteLLM integration, advisory invocation path, provider governance foundation, smoke testing, route stabilization, and operational diagnostics. The follow-on thought is that a new M13B milestone should govern runtime ownership for AI advisory infrastructure, credentials, model selection, and the LiteLLM gateway boundary.

## Raw Input

i am now thinking of a new Milestone M13B AI Gateway Governance And Operational Boundary
M13A established:

LiteLLM integration
advisory invocation
provider governance foundation
smoke testing
route stabilization
operational diagnostics

M13B would establish:

governed AI runtime ownership

Suggested M13B Theme
M13B - AI Gateway Governance And Managed Advisory Runtime

or shorter:

M13B - Managed Advisory Runtime

Suggested Initial Issues
TF-F072 - Global Advisory Model Selection

Focus:

model discovery from LiteLLM
primary/fallback selection
no hardcoded model names
advisory-wide selected model
governance UI redesign

This is the operator experience layer.

TF-F073 - Internalize LiteLLM Gateway Boundary

Focus:

Docker internal networking
remove public LiteLLM exposure
TradeForge-only access
managed advisory gateway semantics

This is the runtime boundary layer.

TF-F074 - Governed LLM Provider Secret Management

Focus:

encrypted Groq/NVIDIA/OpenAI/etc keys
.keys.enc
runtime decryption
runtime injection into LiteLLM
rotation semantics
masking
operational reload

This is the security/governance layer.

And Notice Something Important

These three issues together form a coherent vertical slice:

Operator
  ->
Provider Governance UI
  ->
Encrypted credential governance
  ->
Model selection policy
  ->
TradeForge advisory boundary
  ->
Internal LiteLLM runtime
  ->
Vendor providers

--
WHat follows is chronologically an earlier discussion i had about the subject

TF-F072 - Advisory Prompt Framing And Review Structure

Scope:

structured thesis-review prompt
explicit skepticism framing
operational swing-trading review heuristics
structured output sections
invalidation-focused review
avoid generic filler language

Maybe output sections like:

1. Unsupported Assumptions
2. Missing Evidence
3. Regime Risks
4. Technical Invalidation Risks
5. Sentiment Vulnerabilities
6. Behavioral Risks
7. Questions Before Entry
8. Confidence Gaps

That would massively improve usefulness.

Another Important Realization

You now have proof that:

TradeForge does not need frontier AGI models
to become useful.

Even a tiny local model can:

challenge assumptions
expose gaps
slow impulsive behavior
enforce discipline
force structured thinking

That aligns perfectly with your original philosophy:
TradeForge is not supposed to replace judgment.

It is supposed to improve operator cognition and discipline

I would frame the next issue as:

TF-F072 - Global AI Advisory Model Selection

Scope:

list LiteLLM models from /v1/models
let operator choose primary advisory model
optional fallback model
store selection through encrypted TradeForge credential/config boundary
all advisory tasks use the selected primary/fallback
smoke-test selected model

Out of scope:

per-task routing
automatic model choice
dynamic LiteLLM YAML editing
direct Anthropic/Google SDK support
AI orchestration engine

This is practical and aligned with where the system is today.

Yes - that should absolutely be its own issue.

And importantly, it is NOT merely:

* "docker cleanup"
* or "security hardening."

It is an architectural boundary issue.

Because right now:

```text
Browser/host
    ->
can directly hit LiteLLM :4000
```

But your intended architecture is:

```text
Browser
    ->
TradeForge
    ->
internal AI gateway
```

Meaning LiteLLM should become:

* infrastructure-private
* non-operator-facing
* reachable only through TradeForge.

---

I would create something like:

# TF-F073 - Internalize LiteLLM Gateway Network Boundary

Classification:

* enhancement
* infrastructure / operational governance

Milestone:

* M13A follow-on
  or
* M13B if you create a stabilization sub-milestone.

---

# Problem

LiteLLM is currently exposed on a host port (`localhost:4000`) and can be called independently of TradeForge.

This weakens:

* operational boundary clarity
* credential governance
* advisory routing ownership
* future AI governance architecture

TradeForge should become the sole operator-facing gateway for advisory workflows, while LiteLLM remains an internal infrastructure component reachable only through the Docker network.

---

# Scope

## Include

* remove public host exposure for LiteLLM
* move LiteLLM to internal Docker network only
* TradeForge communicates with LiteLLM through service hostname
* update compose networking
* update advisory configuration defaults
* validate smoke tests through TradeForge only
* ensure Provider Governance continues functioning
* document local dev debugging workflow
* clarify internal gateway architecture in ADR/docs

---

# Acceptance Criteria

* LiteLLM no longer exposed via `localhost:4000`
* browser cannot directly access LiteLLM
* TradeForge advisory workflows continue functioning
* Provider Governance smoke tests continue functioning
* advisory model selection still operational
* Docker internal networking documented
* LiteLLM treated as managed internal advisory gateway

---

# Explicitly Out Of Scope

* dynamic model registry
* automated route orchestration
* per-task routing policies
* Anthropic/OpenAI direct SDK bypass
* distributed inference
* Kubernetes secrets
* vault integration

---

# Important Architectural Point

This issue is really about establishing:

```text
TradeForge owns the advisory boundary.
```

not:

* LiteLLM directly
* not vendor APIs directly
* not raw model invocation directly.

That is a meaningful architectural milestone.

I would NOT put encryption/governed provider secrets entirely inside TF-F072.

That deserves its own issue because it changes:

trust boundaries
runtime secret flow
LiteLLM integration architecture
credential lifecycle semantics

Right now TF-F072 is already becoming:

dynamic model selection + advisory governance UX

Adding:

encrypted downstream LLM vendor secret ownership
runtime injection into LiteLLM
secret rotation semantics
synchronization/reload semantics

will make it too large.

I would split it like this:

TF-F072 - Global Advisory Model Selection

Focus:

model selection UX
primary/fallback model
LiteLLM model discovery
advisory route governance
no hardcoded model names
TF-F073 - Internalize LiteLLM Gateway Boundary

Focus:

Docker networking
internal-only LiteLLM exposure
TradeForge-owned advisory boundary
remove public :4000
TF-F074 - Governed LLM Provider Secret Management

THIS becomes the important credential architecture issue.

TF-F074 Scope
Goal

TradeForge becomes the canonical owner of:

NVIDIA NIM keys
Groq keys
OpenAI keys
Anthropic keys
Google keys

with:

encryption at rest
runtime decryption only at composition boundary
controlled injection into LiteLLM runtime.
Architectural Direction
Operator enters NVIDIA key in Provider Governance UI
        ->
TradeForge encrypts credential into .keys.enc
        ->
TradeForge decrypts only during runtime composition
        ->
TradeForge injects provider secret into managed LiteLLM runtime
        ->
LiteLLM performs vendor call
Important Principle

This keeps the doctrine consistent:

TradeForge owns operational credential governance.

## Observations

- M13A appears to have proven the basic advisory invocation and LiteLLM integration path, but left runtime ownership boundaries unresolved.
- The emerging M13B theme is not just model choice or Docker cleanup; it is governance over the managed advisory runtime.
- The work naturally splits into operator experience, runtime boundary, and security/governance layers.
- TF-F072 was initially considered for prompt framing and review structure, but the later direction reframes it as global advisory model selection.
- The useful role of AI is discipline support: challenge assumptions, expose gaps, slow impulsive behavior, and force structured thinking.
- LiteLLM being directly reachable on `localhost:4000` weakens the intended TradeForge-owned advisory boundary.
- Credential handling is large enough to require a separate issue from model selection.

## Ideas

- Define M13B as `AI Gateway Governance And Managed Advisory Runtime`, or shorter as `Managed Advisory Runtime`.
- Create TF-F072 for global advisory model selection:
  - discover LiteLLM models from `/v1/models`
  - allow a primary advisory model
  - allow an optional fallback advisory model
  - remove hardcoded model names
  - apply the chosen model advisory-wide
  - smoke-test the selected model path
  - redesign provider governance UI around this operator workflow
- Create TF-F073 for internalizing the LiteLLM gateway boundary:
  - remove public host exposure
  - make LiteLLM infrastructure-private on Docker networking
  - require TradeForge-mediated access
  - keep Provider Governance and smoke tests functioning through TradeForge only
  - document the local debugging workflow
- Create TF-F074 for governed LLM provider secret management:
  - TradeForge owns vendor provider keys
  - encrypt keys at rest into `.keys.enc`
  - decrypt only at runtime composition boundaries
  - inject provider secrets into the managed LiteLLM runtime
  - support masking, rotation semantics, and operational reload
- Consider a separate future issue for advisory prompt framing and review structure, including invalidation-focused sections such as unsupported assumptions, missing evidence, regime risks, technical invalidation risks, sentiment vulnerabilities, behavioral risks, questions before entry, and confidence gaps.

## Questions

- Should the milestone name be the fuller `M13B - AI Gateway Governance And Managed Advisory Runtime` or the shorter `M13B - Managed Advisory Runtime`?
- Should the earlier prompt-framing idea keep the TF-F072 number, or should TF-F072 be reserved for global advisory model selection and prompt framing move to a later issue?
- Where should the selected advisory model and fallback model be persisted: encrypted credential/config boundary, event-derived governance state, or another runtime configuration mechanism?
- What exact reload semantics are expected when provider credentials rotate?
- How should local development debug access to LiteLLM work once the public `localhost:4000` exposure is removed?
- Which ADRs and canonical knowledge pages need updates once M13B is processed into formal planning artifacts?

## Concerns

- Combining model selection, gateway internalization, and credential encryption into one issue would make the scope too large.
- Direct browser or host access to LiteLLM bypasses TradeForge as the advisory owner.
- Dynamic YAML editing, per-task routing, direct vendor SDK bypasses, and orchestration-engine behavior may expand the milestone beyond the current operational governance intent.
- Provider secret management affects trust boundaries, runtime secret flow, LiteLLM integration architecture, and credential lifecycle semantics.
- Credential governance must preserve the principle that TradeForge owns operational credential governance while AI remains advisory.

## Possible Next Outputs

- Issue candidates for TF-F072, TF-F073, and TF-F074
- Milestone candidate for M13B
- ADR candidate for TradeForge-owned managed advisory gateway boundary
- Runtime DOCS update for AI gateway governance
- No canonical action until this raw note is processed
