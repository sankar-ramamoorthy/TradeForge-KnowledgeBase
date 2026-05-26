---
title: M13A Provider Governance Roadmap Plan
type: planning-note
status: raw
created: 2026-05-24
tags:
  - tradeforge
  - m13a
  - provider-governance
  - ai-gateway
  - capability-routing
  - credentials
  - operational-ux
  - issue-register
source_synthesis:
  - knowledge/processed/20260523-provider-governance-ai-gateway-configuration-synthesis.md
source_history:
  - knowledge/raw/archived/brainstorm-20260523-provider-governance-ai-gateway-configuration.md
  - knowledge/raw/archived/Brainstorm Notes  about Dedicated Provider configuration v1.md
  - knowledge/raw/archived/Brainstorm Notes  about Dedicated Provider configuration v2.md
  - knowledge/raw/archived/Brainstorm Notes  about Dedicated Provider configuration session 3.md
runtime_files:
  - C:/Users/bosto/dockerstuff/TradeForge/DOCS/Milestone_Roadmap_v2.md
  - C:/Users/bosto/dockerstuff/TradeForge/DOCS/ISSUE_REGISTER.md
---

# M13A Provider Governance Roadmap Plan

## Intent

Promote the processed M13A synthesis into runtime planning by adding a real
`M13A - Provider Governance And AI Gateway Configuration` milestone to Roadmap
v2 and by creating detailed issue-register entries for the work.

M13A is framed as:

```text
External systems governance and AI routing infrastructure.
```

The milestone sits after M13 and before M14 because M10C, M10D, M10E, M11,
M12, and M13 created enough provider, credential, context, and AI advisory
infrastructure that operational governance now needs its own stabilization pass.

## Core Model

The central distinction to preserve is:

```text
Credential != Provider != Capability != Model
```

LiteLLM must be treated as an AI gateway and route boundary, not as a normal
single provider. TradeForge should request semantic advisory roles and
capabilities while gateway configuration maps those roles to concrete model
providers outside workflow logic.

## Runtime Documentation Plan

Update Roadmap v2 with a planned M13A section between M13 and M14. The section
must state that provider governance is an operational control plane, not a
canonical decision workspace. It must also preserve human decision sovereignty,
the AI advisory boundary, replayability, and the distinction between canonical
truth and derived/advisory external-system state.

Update the local issue register with detailed M13A issue entries using the
`TF-F059+` feedback issue series.

## Issue Set

- `TF-F059: Formalize M13A provider governance roadmap`
- `TF-F060: Define provider governance control surface design`
- `TF-F061: Define capability routing governance model`
- `TF-F062: Define AI gateway and route alias model`
- `TF-F063: Define provider diagnostics and health history model`
- `TF-F064: Implement provider governance read APIs`
- `TF-F065: Implement credential validation and test workflow`
- `TF-F066: Implement AI gateway route visibility`
- `TF-F067: Implement provider governance frontend surface and rail cleanup`
- `TF-F068: M13A verification and M14 readiness gate`

## Explicit Boundaries

M13A does not authorize autonomous trading, AI decision authority, AI plan
approval, broker execution automation, a generalized AI orchestration engine, or
canonical event-ledger writes from provider health, gateway state, or model
output.

Contextual rails should become status and provenance surfaces. Long-form
credential management, route management, model selection, and diagnostics belong
in the provider governance control surface.

## Implementation Note

This raw plan records the planning pass itself. Runtime implementation remains
tracked through the M13A issue set. The first runtime documentation pass should
also revert the accidental earlier frontend change that mounted
`ProviderConfigurationPanel` on the Operating rail, because that change solved
the wrong level of the problem.
