---
title: Provider Capability Gap Before M11
type: brainstorm
status: raw
tags: [trading-system, brainstorm, provider-capability, external-data, m10d]
created: 2026-05-16
---

# Provider Capability Gap Before M11

## Trigger

Credential-boundary work made provider setup explicit before M11, but the active runtime provider model still describes only the normalized OHLCV snapshot path.

## Raw Input

Current runtime supports only a normalized price snapshot path.
UI does not expose provider selection.
Credential setup names providers whose actual capabilities differ materially.
Future AI work risks consuming an underspecified provider layer.
Candidate direction: provider registry plus typed capability contracts.

Open questions to preserve:
- fallback ordering
- concrete first-rollout providers
- how much operator control vs automation
- how capability metadata should appear in workspaces

## Observations

- The implemented provider boundary is strong for OHLCV market snapshots but narrow in scope.
- Provider identity is currently easy to conflate with price-feed behavior.
- Credential documentation already references providers expected to support materially different external-data surfaces.
- The UI does not currently expose which provider serves which external-data need.
- Beginning M11 on top of the current shape would force advisory work to infer provider semantics that the architecture has not stated explicitly.

## Ideas

- Add a small provider registry as the configured catalog of provider identities and supported capabilities.
- Put typed capability contracts behind the registry rather than widening one generic provider interface indefinitely.
- Start with `price` and `fundamentals` only.
- Keep provider provenance visible to operators and workspace consumers.
- Preserve the existing normalized price snapshot boundary while extending the broader provider model around it.

## Questions

- Should M10D model only a preferred provider per capability, or also ordered fallback resolution?
- Which fundamentals providers belong in the first rollout: all documented providers, one anchor provider, or one primary plus one fallback?
- Should operator-facing configuration be read-only transparency first, editable configuration, or editable configuration plus health/status?
- Where should fundamentals overlays appear first: Opportunity only, Opportunity plus Thesis/Plan flows, or all decision workspaces?

## Concerns

- Generalizing too early could create framework complexity without operational need.
- Delaying the distinction between provider identity and provider capability would leave later consumers coupled to an OHLCV-shaped mental model.
- Any new external-data path must remain advisory, provenance-bearing, and clearly separate from canonical event-ledger truth.

## Possible Next Outputs

- Issue candidate
- ADR candidate
- Topic page update after semantics stabilize
