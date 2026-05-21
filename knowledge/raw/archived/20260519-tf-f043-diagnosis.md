---
title: TF-F043 Diagnosis - FMP Stable Fundamentals Endpoints
type: diagnosis
status: raw
tags: [TradeForge, TF-F043, feedback, fmp, fundamentals, provider-adapter]
created: 2026-05-19
---

# TF-F043 Diagnosis - FMP Stable Fundamentals Endpoints

## Source Feedback

- `knowledge/raw/brainstorm-20260519-fmp-stable-endpoints-and-quote-context.md`
- Runtime observation: `/workspaces/fundamentals-context?symbol=NVDA&instrument_kind=equity` returned unavailable fundamentals.
- Provider attempts showed `fmp: HTTP Error 403: Forbidden` followed by an Alpha Vantage fallback shape failure.

## Observable Gap

The operator can access FMP fundamentals data through the newer stable endpoint:

```text
https://financialmodelingprep.com/stable/income-statement?symbol=AAPL&apikey=<key>
```

but the runtime still reports fundamentals unavailable because its FMP adapter calls the older route family:

```text
https://financialmodelingprep.com/api/v3/profile/{symbol}
https://financialmodelingprep.com/api/v3/income-statement/{symbol}
https://financialmodelingprep.com/api/v3/ratios/{symbol}
```

## Root Cause

The FMP adapter endpoint family is stale relative to the credential entitlement
that works in the current environment. The provider key is valid for the stable
API route, but the runtime adapter is asking the provider through older paths
that can return authorization failures.

## Relevant Invariants

- Market Intelligence Is Interpreted Context: provider fundamentals are advisory external context.
- Derived State Must Remain Distinguishable: normalized fundamentals must not become canonical ledger truth.
- Architectural Simplicity: fix the adapter route mapping without broadening the provider model.

## Minimum Correct Scope

- Update the FMP fundamentals adapter to call stable profile, income statement, and ratios endpoints.
- Update normalization to match stable response field names where they differ.
- Add or update adapter tests so the expected stable URLs and response shapes are covered.

## Explicitly Out Of Scope

- Adding FMP quote fields to runtime models.
- Modeling market cap or moving averages from the quote endpoint.
- Changing provider fallback semantics.
- Changing lifecycle, event, replay, or workspace authority.

## ADR Checkpoint

No new ADR is required. This is a bug fix within the provider adapter implementation
already governed by ADR-0038. It does not introduce a new capability family,
bounded context, lifecycle state, or event model change.
