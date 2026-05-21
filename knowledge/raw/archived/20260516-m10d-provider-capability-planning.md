---
title: M10D Provider Capability Implementation Planning
type: brainstorm
status: raw
tags: [tradeforge, m10d, provider-capability, planning]
created: 2026-05-16
---

# M10D Provider Capability Implementation Planning

## Issue Scope

Implement TF-F017 through TF-F023 after TF-F016 stabilized the architecture.

## Design Plan

- Keep ADR-0032 price snapshots intact.
- Add provider identity/capability metadata above existing adapters.
- Introduce a registry with deterministic preferred-plus-fallback resolution.
- Add typed fundamentals contracts beside price contracts.
- Use `fmp` primary and `alpha_vantage` fallback for first rollout.
- Keep all external data advisory and non-canonical.
- Surface configuration and provenance to operators.
- Place fundamentals overlays in Opportunity and Thesis flows only.

## Event / Lifecycle Impact

- No canonical event additions.
- No lifecycle transition changes.
- Provider resolution remains advisory context only.

## Replay Impact

- Provider selection and fallback state must remain explicit so future replay can reconstruct what external context was visible.

## Verification Plan

- Registry resolution tests.
- Fundamentals service fallback tests.
- Adapter normalization and failure tests.
- Overlay/API regression tests.
- Existing price path non-regression tests.
- Frontend build verification.
