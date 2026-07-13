---
title: TF-F079 Synthesis - Alpha Vantage Partial Fundamentals
type: processed-synthesis
status: processed
created: 2026-07-13
updated: 2026-07-13
tags: [TradeForge, TF-F079, feedback, provider-boundary, alpha-vantage, fundamentals]
source_history:
  - knowledge/raw/archived/brainstorm-20260713-alpha-vantage-partial-fundamentals.md
related:
  - "[[Provider Boundary Model]]"
  - "[[M10D Synthesis - Provider Capability Model and Architecture]]"
issues:
  - TF-F079
---

# TF-F079 Synthesis - Alpha Vantage Partial Fundamentals

## What The Feedback Identified

Live Swagger testing showed that TradeForge could report Alpha Vantage
fundamentals as fully unavailable even when the upstream `OVERVIEW` function
returned usable company data. The failing path surfaced only
`alpha_vantage: 'annualReports'`, which hid both the upstream function boundary
and the fact that another provider payload had already succeeded.

## Root Cause

The Alpha Vantage fundamentals adapter treated `OVERVIEW` and
`INCOME_STATEMENT` as one all-or-nothing fetch. A missing or malformed
`annualReports` value from `INCOME_STATEMENT` raised before the adapter could
return profile and ratio data from `OVERVIEW`.

## Change Made

The runtime fix:

- validates `OVERVIEW` independently
- returns partial normalized fundamentals from overview profile and ratio
  fields when income statements are unavailable
- represents missing statement facts as an empty statement collection, which
  the existing API already maps to null revenue and net income
- improves unusable-overview diagnostics so they name `OVERVIEW` and include
  upstream Alpha Vantage messages where present
- adds tests proving the adapter sends `OVERVIEW` first and
  `INCOME_STATEMENT` second

## Provider Boundary Meaning

FMP and Alpha Vantage may return materially different shapes. That difference
is expected and belongs inside adapter-specific normalization, not in shared
domain or API code. The shared fundamentals contract remains the normalized
boundary: profile, statements, ratios, and provenance.

Partial provider success is still advisory derived context. It does not create
canonical truth, lifecycle authority, or execution permission.

## Invariants

Relevant invariants:

- [[Market Intelligence Is Interpreted Context]]
- [[Derived State Must Remain Distinguishable]]
- [[Replayability Is Foundational]]
- [[UX Is Architectural]]

The change preserves replay and lifecycle boundaries. Live provider calls
remain runtime context acquisition only; replay reconstruction must continue to
use persisted historical context rather than current Alpha Vantage responses.

## Follow-Up Consideration

Provider diagnostics could eventually show redacted request shape and per-call
payload status. That would answer the operator's "what did the system send?"
question without exposing credentials.

## Closure

TF-F079 closes when a usable Alpha Vantage `OVERVIEW` response returns
available advisory fundamentals even if `INCOME_STATEMENT` lacks
`annualReports`, with revenue and net income left null rather than making the
whole context unavailable.
