---
title: M10D Synthesis — Provider Capability Model and Architecture
type: processed-synthesis
status: processed
milestone: M10D
created: 2026-05-16
source_history:
  - knowledge/raw/20260516-m10d-provider-capability-planning.md
  - knowledge/raw/20260516-m10d-provider-capability-implementation.md
  - knowledge/raw/ArchitecturalDoctrineDecisionsForM10D.md
  - knowledge/raw/WhichPrimaryproviderAndWhichFallbackOneandWhyForM10D.md
  - knowledge/raw/brainstorm-20260516-provider-capability-gap-before-m11.md
tags: [M10D, provider-capability, market-data, advisory, registry, fundamentals, FMP]
related:
  - "[[Provider Boundary Model]]"
  - "[[Market Intelligence Is Interpreted Context]]"
  - "[[Derived State Must Remain Distinguishable]]"
---

# M10D Synthesis — Provider Capability Model and Architecture

## Stabilized Understanding

M10D extends the M9 provider boundary model from a single normalized OHLCV path to a typed capability registry that supports multiple external-data surfaces (price, fundamentals) with preferred-plus-ordered-fallback resolution.

## Architectural Doctrine Decisions Preserved

Four doctrine decisions were identified before implementation:

**1. Fundamentals rollout set:** One primary + one fallback. Tests normalization discipline early, surfaces routing philosophy, keeps complexity manageable. Avoid all-providers-at-once or single-provider assumptions.

**2. Capability resolution model:** Preferred + ordered fallback (deterministic, explicit). Provider selection must be auditable and replayable — not opaque.

**3. Provider identity:** Provider registry holds the configured catalog of provider identities and supported capabilities. Provider identity is separated from capability contracts to prevent conflation of "who provides it" with "what it provides."

**4. Fundamentals overlays:** Opportunity and Thesis flows only for first rollout. Keeps scope manageable while covering the highest-value decision surfaces.

## Provider Selection (FMP + Alpha Vantage)

FMP chosen as primary for M10D because its broad capability surface (price, fundamentals, statements, ratios, profiles, earnings, sector/industry) reinforces the architectural direction without overfitting to an OHLCV-only mental model. Alpha Vantage as fallback for fundamentals capability divergence testing.

## Key Architectural Constraints Maintained

- All external data remains advisory and non-canonical.
- No canonical event additions. No lifecycle transition changes.
- Provider resolution stays advisory context only.
- Existing normalized price snapshot boundary (ADR-0032) preserved; capability model extends around it.
- Provenance remains visible to operators and workspace consumers.

## Capability Gap Insight (brainstorm note)

Before M10D, provider identity was easy to conflate with price-feed behavior — the UI didn't expose which provider served which external-data need. M10D addresses this by making provider capabilities explicit in a registry before AI advisory work (M11) begins consuming them.
