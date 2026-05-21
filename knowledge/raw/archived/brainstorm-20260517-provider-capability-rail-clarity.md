---
title: Provider Capability Controls Are Visually Conflated In Context Rail
type: brainstorm
status: raw
tags: [tradeforge, brainstorm, provider-capabilities, frontend, operator-friction]
created: 2026-05-17
---

# Provider Capability Controls Are Visually Conflated In Context Rail

## Trigger

Operator changed the visible fundamentals provider selection while inspecting Market Context in the runtime UI.

## Raw Input

```text
Market Context

Advisory price data from market provider. Non-authoritative — does not affect lifecycle state or canonical truth.
INTC High Vol Advisory
O109.79000091552734 H110.56999969482422 L105.0199966430664 C108.7699966430664 Vol134,499,900
yfinance · data as of May 15, 2026

Provider Configuration
Advisory
Fundamentals provider

Selected: alpha_vantage / fallback order: fmp
The load button still just gets data from yFinance
```

## Observations

- The visible `Load` action belongs to the price-data Market Context panel.
- The visible provider selector below it belongs to the fundamentals capability.
- The rail does not expose the price capability configuration, so the operator has no immediate visual signal that two separate external-data paths are present.
- The resulting UI suggests a false relationship between the fundamentals selector and the price-data loader.

## Ideas

- Surface both `price` and `fundamentals` capability resolution in the provider panel.
- Keep the existing fundamentals selector editable, but show the selected price provider beside it.
- Preserve the typed price/fundamentals split rather than merging the controls.

## Questions

- Should future work make price provider selection editable through the same panel once dynamic price-provider routing exists?
- Should provider configuration remain in the rail on workspaces that do not render fundamentals overlays?

## Concerns

- Capability-specific architecture is already correct, but the UI hides that distinction at the point of use.
- When advisory data surfaces are visually conflated, the operator may draw the wrong conclusion about which provider supplied which context.
- The bug weakens the transparency promised by the provider-capability milestone without requiring any backend semantic change.

## Possible Next Outputs

- Feedback issue candidate
- Context-rail clarity fix
- Follow-on issue for dynamic price-provider selection if needed
