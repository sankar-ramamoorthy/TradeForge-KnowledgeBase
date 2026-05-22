---
title: TF-F027 Diagnosis - Provider Capability Rail Clarity
type: brainstorm
status: raw
tags: [tradeforge, tf-f027, diagnosis, frontend, provider-capabilities]
created: 2026-05-17
---

# TF-F027 Diagnosis - Provider Capability Rail Clarity

## Observable Gap

The context rail shows a `Market Context` load action that returns `yfinance` price data and, directly below it, a `Provider Configuration` panel that only exposes the fundamentals provider. Changing the visible fundamentals setting makes the neighboring load action appear broken.

## Root Cause

The frontend correctly keeps price and fundamentals on separate typed paths, but the rail surfaces only the fundamentals capability in its configuration panel. Because the selected price provider is omitted, the UI visually conflates the two capabilities and hides which provider the price loader actually uses.

## Relevant Invariants

- UX Is Architectural: the interface must preserve operational clarity.
- Derived State Must Remain Distinguishable: separate advisory data families should remain explicit rather than visually collapsed.

## Minimum Correct Scope

Expose both price and fundamentals capability resolution in the provider panel while keeping existing backend contracts and the separate market-context/fundamentals-context flows intact.

## Out Of Scope

- dynamic multi-provider routing for price data
- changing provider rollout doctrine
- broad workspace redesign
