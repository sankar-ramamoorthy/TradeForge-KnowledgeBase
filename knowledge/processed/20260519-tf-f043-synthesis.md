---
title: TF-F043 Synthesis - FMP Stable Fundamentals Endpoint Fix
type: processed-synthesis
status: processed
tags: [TradeForge, TF-F, feedback, fmp, fundamentals, provider-adapter]
created: 2026-05-19
updated: 2026-05-19
source_history:
  - knowledge/raw/brainstorm-20260519-fmp-stable-endpoints-and-quote-context.md
  - knowledge/raw/20260519-tf-f043-diagnosis.md
  - knowledge/raw/20260519-tf-f043-planning.md
related:
  - "[[Market Intelligence Is Interpreted Context]]"
  - "[[Derived State Must Remain Distinguishable]]"
issues:
  - TF-F043
---

# TF-F043 Synthesis - FMP Stable Fundamentals Endpoint Fix

## Feedback Identified

The operator observed that FMP's newer stable endpoints return usable data with
the configured local key, while TradeForge reported fundamentals as unavailable.
The operator also noted that FMP stable quote returns potentially useful future
context such as market cap and 50/200 moving-average style fields.

## Root Cause

The runtime FMP fundamentals adapter used older `/api/v3/...` endpoint routes.
Live testing showed the configured credential could access `/stable/...` routes,
but the older routes could return authorization failures. The provider credential
was not the immediate problem; the adapter endpoint family was stale.

## Change Made

The FMP fundamentals adapter now calls:

- `/stable/profile`
- `/stable/income-statement`
- `/stable/ratios`

with `symbol` and `apikey` query parameters. The adapter still returns the
existing advisory `FundamentalsBundle` contract and preserves provider
provenance. Stable ratios normalization now reads `priceToEarningsRatio`; return
on equity remains optional when no equivalent stable field is present in the
observed ratios payload.

## Invariants Involved

- Market intelligence remains advisory interpreted context.
- Normalized fundamentals remain derived external context, not event-ledger truth.
- The fix stays inside the infrastructure adapter and does not alter lifecycle authority.

## Replay Or Lifecycle Implications

No event model, replay model, lifecycle transition, or workspace authority change
was made. Provider data remains non-canonical and read-only.

## Feedback Closure

The original unavailable-fundamentals observation was verified against the fixed
adapter with a live FMP stable check for `NVDA`. The fixed adapter returned
company name, sector, statement date, revenue, net income, and P/E from provider
`fmp`.

After restarting the local API container, the runtime endpoint
`GET /workspaces/fundamentals-context?symbol=NVDA&instrument_kind=equity`
returned `coverage_status: available`, selected provider `fmp`, and a successful
provider attempt.

The FMP stable quote observation was captured for future evaluation but left out
of TF-F043 to avoid mixing a provider-route bug fix with a new context-model
enhancement.
