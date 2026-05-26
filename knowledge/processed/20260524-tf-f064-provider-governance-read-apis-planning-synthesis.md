---
title: TF-F064 Provider Governance Read APIs Planning Synthesis
type: processed-synthesis
status: processed
created: 2026-05-24
source_raw:
  - knowledge/raw/20260524-tf-f064-provider-governance-read-apis-planning.md
source_issue: TF-F064
milestone: M13A
tags:
  - TradeForge
  - M13A
  - provider-governance
  - read-api
---

# TF-F064 Provider Governance Read APIs Planning Synthesis

TF-F064 implements the first runtime-facing provider governance read model for M13A. The endpoint is intentionally operational and advisory: it reports current provider, credential, route, diagnostic, and AI gateway state without creating canonical decision facts.

## Stabilized Design

The read API composes existing runtime authority rather than introducing a new domain authority:

- Provider identity and capability support come from the provider registry.
- Capability route resolution remains deterministic preferred-plus-ordered-fallback.
- Credential status comes from the credential store, but response bodies must not include plaintext, masked, or display credential field values.
- AI gateway status is reported as LiteLLM gateway state and advisory boundary metadata.
- Diagnostics are ephemeral/current summaries unless a future issue implements explicit retention.

## Runtime Boundary

The route belongs at app/runtime governance level, not inside a decision workspace. `/provider-governance` is therefore preferred over adding more workspace rail-oriented endpoints. Existing provider configuration routes remain available for compatibility.

## Invariant Alignment

- Derived State Must Remain Distinguishable: response uses operational/advisory authority and `is_canonical=false`.
- AI Advisory Boundary: gateway metadata cannot imply lifecycle, approval, execution, or buy/sell authority.
- Human Decision Sovereignty: provider state supports operator awareness only.
- Replayability: live provider checks are not used to reconstruct historical route or health state.

## ADR Result

No new ADR is needed. The implementation follows existing ADRs and the M13A design docs.
