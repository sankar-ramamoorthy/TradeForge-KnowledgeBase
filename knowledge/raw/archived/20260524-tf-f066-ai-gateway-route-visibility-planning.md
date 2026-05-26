---
title: TF-F066 AI Gateway Route Visibility Planning
type: raw-planning
status: raw
created: 2026-05-24
source_issue: TF-F066
milestone: M13A
tags:
  - TradeForge
  - M13A
  - ai-gateway
  - runtime-planning
---

# TF-F066 AI Gateway Route Visibility Planning

## Issue Discipline

- Issue ID: TF-F066
- Milestone: M13A
- Title: Implement AI Gateway Route Visibility
- Affected layers: infrastructure/advisory, app, frontend
- Linked ADRs: ADR-0006, ADR-0037, ADR-0041, ADR-0042
- Impacted invariants: AI Advisory Boundary, Human Decision Sovereignty, Derived State Must Remain Distinguishable, UX Is Architectural
- Depends on: TF-F062, TF-F064

## Design Reasoning

TF-F064 introduced a provider governance overview with static TradeForge-facing route aliases. TF-F066 should deepen the AI gateway portion so operators can understand LiteLLM as a gateway and route boundary, not as an ordinary one-key provider.

The implementation should expose safe gateway metadata from the LiteLLM credential shape:

- gateway URL
- default model/route target
- inferred underlying provider where the model uses a provider/model prefix
- TradeForge route aliases and advisory usage domains
- route availability as configured/unconfigured operational metadata
- reachability state without generating model output

The default read path should avoid hidden generation calls. Reachability can be represented as current known state or a non-token health probe where explicitly requested or already available through advisory health.

## Event, Lifecycle, And Replay Impact

- No new event types.
- No lifecycle authority.
- AI gateway metadata is operational/advisory state only.
- Replay must not call LiteLLM to reconstruct historical route availability.

## Implementation Plan

1. Extend provider governance AI gateway response with gateway URL, default model, inferred underlying provider, and boundary fields.
2. Add a focused AI gateway read endpoint under provider governance for frontend surfaces that need just gateway details.
3. Reuse LiteLLM credential decryption for non-secret metadata only; never return API keys.
4. Keep route aliases semantic and advisory-role based.
5. Represent reachability as `not_checked` in the read path, preserving the existing `/advisory/health` endpoint for explicit health checks.
6. Add tests for configured gateway metadata, unconfigured gateway metadata, no secret leakage, and route alias role visibility.
7. Sync roadmap/register and capture implementation knowledge.

## ADR Checkpoint

No new ADR is required. ADR-0006, ADR-0037, ADR-0041, ADR-0042, and TF-F062 already establish the AI advisory and LiteLLM gateway boundaries. TF-F066 adds bounded read visibility.
