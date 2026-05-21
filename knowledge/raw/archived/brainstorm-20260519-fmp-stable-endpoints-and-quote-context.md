---
title: FMP Stable Endpoints And Quote Context Observation
type: brainstorm
status: raw
tags: [TradeForge, brainstorm, fmp, fundamentals, provider-context]
created: 2026-05-19
---

# FMP Stable Endpoints And Quote Context Observation

## Trigger

During investigation of why TradeForge showed fundamentals as unavailable, the
operator tested FinancialModelingPrep stable endpoints directly and observed
that the newer stable API path returns usable data with the configured key.

## Raw Input

The operator noted that:

```text
https://financialmodelingprep.com/stable/income-statement?symbol=AAPL&apikey=
```

works when sent with the correct local key.

The operator also noticed that:

```text
https://financialmodelingprep.com/stable/quote?symbol=AAPL&apikey=....
```

returns useful quote context, including:

```json
[
  {
    "symbol": "AAPL",
    "name": "Apple Inc.",
    "price": 297.84,
    "changePercentage": -0.79606,
    "change": -2.39,
    "volume": 34313641,
    "dayLow": 294.91,
    "dayHigh": 300.66,
    "yearHigh": 303.2,
    "yearLow": 193.46,
    "marketCap": 4374482111040,
    "priceAvg50": 266.1892,
    "priceAvg200": 259.13245,
    "exchange": "NASDAQ",
    "open": 300.24,
    "previousClose": 300.23,
    "timestamp": 1779134401
  }
]
```

The operator observed that this is good information because it includes market
capitalization and 50/200 moving average values, while noting uncertainty about
the exact moving-average period semantics.

## Observations

- The current runtime FMP fundamentals adapter used older `/api/v3/...` routes.
- A direct test of `/stable/income-statement` succeeded with the configured FMP key.
- A direct test of `/stable/quote` returned price, market cap, 50-day average, and 200-day average style fields.
- Quote fields may be useful later for richer advisory context, but they are not part of the current fundamentals contract.

## Ideas

- Update the current FMP fundamentals adapter to use the stable fundamentals endpoints.
- Preserve the quote endpoint observation as a future context-family or price-advisory enhancement candidate.
- Consider whether `marketCap`, `priceAvg50`, and `priceAvg200` belong in price context, fundamentals context, or a separate instrument summary overlay.

## Questions

- What exact period definitions does FMP attach to `priceAvg50` and `priceAvg200`?
- Should quote-derived market cap be modeled as price context, fundamentals context, or instrument reference context?
- Should TradeForge retain the existing fundamentals fields only for now and defer quote enrichment to a separate issue?

## Concerns

- Expanding the current fix to include quote fields would mix a provider-route bug fix with a new data-model enhancement.
- Quote data remains advisory external context and must not become canonical lifecycle truth.
- Moving-average semantics need provider documentation or explicit provenance before being surfaced as interpreted context.

## Possible Next Outputs

- Issue candidate: fix FMP fundamentals adapter to use stable endpoints.
- Issue candidate: evaluate FMP stable quote as future advisory quote/instrument context.
- No immediate ontology promotion unless quote context becomes a stabilized runtime concept.
