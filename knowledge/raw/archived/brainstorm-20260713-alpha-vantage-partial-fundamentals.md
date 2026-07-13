---
title: Alpha Vantage Partial Fundamentals Feedback
type: brainstorm
status: raw
tags: [TradeForge, brainstorm, provider-boundary, alpha-vantage, fundamentals]
created: 2026-07-13
---

# Alpha Vantage Partial Fundamentals Feedback

## Trigger

Operator tested the configured Alpha Vantage fundamentals path through Swagger
after adding provider credentials.

## Raw Input

The operator observed that direct Alpha Vantage calls worked for:

- `OVERVIEW`
- `INCOME_STATEMENT`

However, TradeForge returned unavailable fundamentals for:

```text
GET /workspaces/fundamentals-context?symbol=INTC&instrument_kind=equity
```

The returned response included:

```text
coverage_status: unavailable
attempted_provider_ids: ["alpha_vantage"]
errors: ["alpha_vantage: 'annualReports'"]
```

Follow-up operator thought:

```text
we are not 100% sure what the curl command was that the system sent to alpha
vantage. maybe it had an error. maybe if the first one worked it should use
that data and not just error out when the second one fails.
```

The operator also noted that FMP and Alpha Vantage may return different
formats, and now has an FMP API key available to configure after the fix.

## Observations

- Alpha Vantage `OVERVIEW` contains usable company profile and ratio fields.
- Alpha Vantage `INCOME_STATEMENT` is a separate provider function with a
  different payload shape and failure surface.
- A missing `annualReports` key should not necessarily invalidate the usable
  overview fields.
- The surfaced error was a Python key lookup, not a provider-aware diagnostic.
- The operator had no immediate visibility into the exact Alpha Vantage query
  shape used by the adapter.

## Ideas

- Treat `OVERVIEW` and `INCOME_STATEMENT` as independent provider payload
  sections during normalization.
- Return partial normalized fundamentals when the overview is usable but the
  statement payload is missing, throttled, or malformed.
- Preserve profile and ratio fields while leaving revenue and net income null.
- Add regression coverage for the exact provider functions requested.
- Keep FMP and Alpha Vantage as separate adapters because format differences
  belong inside adapter-specific normalization.

## Questions

- Should provider diagnostics eventually expose a redacted upstream request
  shape in runtime health output?
- Should unavailable secondary sections be surfaced to the operator as a
  degraded/partial detail, even when the top-level context is available?
- Should FMP become the preferred provider whenever both FMP and Alpha Vantage
  credentials are configured?

## Concerns

- Provider throttling messages and malformed payloads can look like internal
  runtime failures if diagnostics are too opaque.
- Returning unavailable context when partial context exists creates unnecessary
  operational friction.
- Capturing full provider URLs must avoid leaking credentials.

## Possible Next Outputs

- Issue candidate: TF-F079
- Processed synthesis about partial provider normalization
- Provider diagnostics follow-up
- No ADR unless provider routing policy changes
