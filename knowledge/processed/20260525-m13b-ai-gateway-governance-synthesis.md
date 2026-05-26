---
title: M13B AI Gateway Governance And Managed Advisory Runtime Synthesis
type: processed-synthesis
status: processed
created: 2026-05-25
source:
  - knowledge/raw/brainstorm-20260525-m13b-ai-gateway-governance.md
archived_source:
  - knowledge/raw/archived/brainstorm-20260525-m13b-ai-gateway-governance.md
tags:
  - m13b
  - ai-gateway-governance
  - managed-advisory-runtime
  - litellm
  - credential-governance
  - provider-governance
---

# M13B AI Gateway Governance And Managed Advisory Runtime Synthesis

## Source

Processed from:

```text
knowledge/raw/brainstorm-20260525-m13b-ai-gateway-governance.md
```

Archived after promotion to:

```text
knowledge/raw/archived/brainstorm-20260525-m13b-ai-gateway-governance.md
```

## Synthesis

M13A established the first working provider-governance and LiteLLM advisory
gateway foundation: advisory invocation, route visibility, diagnostics,
explicit smoke testing, and frontend/API surfaces.

The raw M13B brainstorm identifies the next governance gap. The issue is not
only model choice, Docker port exposure, or credential cleanup. The broader
concept is managed advisory runtime ownership:

```text
Operator
  -> TradeForge Provider Governance
  -> encrypted LLM provider credential governance
  -> global advisory model selection
  -> TradeForge advisory boundary
  -> internal LiteLLM runtime
  -> vendor model providers
```

M13B therefore stabilizes a vertical slice across operator experience,
runtime boundary, and security governance.

## Current State

The M13B promotion has now been reflected in runtime planning and
implementation artifacts:

- `DOCS/Milestone_Roadmap_v2.md` includes an M13B section between M13A and M14.
- `DOCS/ISSUE_REGISTER.md` contains TF-F072, TF-F073, and TF-F074 as done
  M13B issues.
- `DOCS/adr/0043-managed-ai-gateway-and-governed-llm-provider-secrets.md`
  records the accepted boundary change.

The KB synthesis remains the canonical explanation of why M13B exists and what
boundary it stabilized. The runtime docs now carry the execution state.

## Planning Decision

Promote M13B into runtime planning as:

```text
M13B - AI Gateway Governance And Managed Advisory Runtime
```

Create three planned runtime issues:

- `TF-F072 - Global Advisory Model Selection`
- `TF-F073 - Internalize LiteLLM Gateway Network Boundary`
- `TF-F074 - Governed LLM Provider Secret Management`

Create required ADR:

- `ADR-0043 - Managed AI Gateway And Governed LLM Provider Secrets`

## Issue Breakdown

### TF-F072 - Global Advisory Model Selection

Purpose:

- discover LiteLLM models/routes through TradeForge
- allow an operator-selected primary advisory model/route
- allow an optional fallback model/route
- remove hardcoded advisory model names
- apply the selected configuration advisory-wide
- smoke-test the selected route through TradeForge

Scope boundary:

- no automatic hidden model choice
- no per-task routing policies
- no generalized orchestration
- no direct vendor SDK bypass

### TF-F073 - Internalize LiteLLM Gateway Network Boundary

Purpose:

- remove public LiteLLM host exposure by default
- make LiteLLM Docker-internal managed infrastructure
- require TradeForge-mediated advisory access
- keep Provider Governance route visibility and smoke tests working through
  TradeForge
- document local debugging workflow

Scope boundary:

- no distributed inference infrastructure
- no Kubernetes or platform hardening
- no lifecycle or event model changes

### TF-F074 - Governed LLM Provider Secret Management

Purpose:

- make TradeForge the governed owner of downstream LLM provider secrets
- store Groq, NVIDIA NIM, OpenAI, Anthropic, Google-style keys encrypted at
  rest in `.keys.enc`
- mask secrets in UI/API responses
- decrypt only at the runtime composition boundary
- define managed injection into LiteLLM
- define rotation and reload semantics

Scope boundary:

- no storage of `TRADEFORGE_MASTER_KEY` in UI or Git
- no external vault integration
- no broker credential changes
- no direct vendor SDK bypass

## ADR Implication

M13B requires a new ADR because it changes an accepted M13A boundary.

M13A said TradeForge stores only LiteLLM gateway credentials, while downstream
model-provider keys live in LiteLLM config or operator environment unless a
future issue changes that boundary.

M13B is that future issue set. TradeForge becomes the governed owner of
downstream LLM provider secrets for the managed advisory runtime.

The ADR must preserve:

- AI advisory-only authority
- human decision sovereignty
- replay/provenance clarity
- composition-boundary decryption
- LiteLLM as internal managed infrastructure

## Deferred Candidate

The earlier prompt-framing idea should not consume `TF-F072`.

Preserve it as a deferred M14 candidate:

```text
TF-F075 - Advisory Prompt Framing And Review Structure
```

Candidate scope:

- structured thesis-review prompts
- skepticism framing
- invalidation-focused review
- sections for unsupported assumptions, missing evidence, regime risks,
  technical invalidation risks, sentiment vulnerabilities, behavioral risks,
  questions before entry, and confidence gaps

Do not add TF-F075 to the current issue register as part of M13B promotion.

## Semantic Boundaries

M13B does not authorize:

- autonomous trading
- AI plan approval
- lifecycle mutation by AI
- hidden model choice
- automatic advisory generation
- direct raw model invocation from browser workflows
- AI output as canonical event truth

M13B reinforces that AI is useful because it can improve operator cognition,
challenge assumptions, expose gaps, slow impulsive behavior, and force
structured thinking while leaving judgment with the human operator.

## Promotion Output

Runtime artifacts created from this synthesis:

- `DOCS/Milestone_Roadmap_v2.md` gains one M13B section between M13A and M14.
- `DOCS/ISSUE_REGISTER.md` gains planned entries for TF-F072, TF-F073, and
  TF-F074.
- `DOCS/adr/0043-managed-ai-gateway-and-governed-llm-provider-secrets.md`
  records the accepted boundary change.

## Canonical Readiness

This synthesis is planning-ready, not doctrine-wide canonical truth.

The durable implementation authority lives in the runtime roadmap, issue
register, and ADR. Broader canonical doctrine updates should wait until M13B
implementation verifies the exact runtime shape.
