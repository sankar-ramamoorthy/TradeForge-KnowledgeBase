---
title: TF-F027 Synthesis - Provider Capability Rail Clarity
type: processed-synthesis
status: processed
tags: [TradeForge, TF-F, feedback, frontend, provider-capabilities]
created: 2026-05-17
updated: 2026-05-17
source_history:
  - knowledge/raw/brainstorm-20260517-provider-capability-rail-clarity.md
  - knowledge/raw/20260517-tf-f027-diagnosis.md
  - knowledge/raw/20260517-tf-f027-planning.md
related:
  - "[[UX Is Architectural]]"
  - "[[Derived State Must Remain Distinguishable]]"
issues:
  - TF-F027
---

# TF-F027 Synthesis - Provider Capability Rail Clarity

## What Feedback Identified

The operator could see and edit the fundamentals provider, but the neighboring Market Context loader still returned `yfinance` price data, making the UI appear inconsistent.

## Root Cause

The frontend exposed only the fundamentals capability in the rail while omitting the price capability used by the Market Context loader. The typed provider architecture was correct; the operator-facing transparency was incomplete.

## What Changed

- Expanded the provider configuration panel to show both price and fundamentals capability resolution.
- Kept the fundamentals selector editable.
- Made the selected price provider visible beside the price-data loader.

## Why This Scope Was Correct

The problem was a misleading presentation gap, not a failure of provider normalization or credential wiring. Showing both capability paths resolves the observed confusion without widening the backend architecture.

## Invariants

- UX Is Architectural
- Derived State Must Remain Distinguishable

## Replay / Lifecycle Impact

None.

## Closure

The context rail now reveals that the load button serves the price capability while the fundamentals selector controls a different capability, closing the original observation without collapsing the two advisory data paths.
