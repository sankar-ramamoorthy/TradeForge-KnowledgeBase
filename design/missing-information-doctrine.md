---
title: Missing Information Doctrine
status: canonical
type: design-doctrine
created: 2026-05-17
updated: 2026-05-17
tags:
  - TradeForge
  - design
  - ux
  - uncertainty
  - missing-information
related:
  - UX_DOCTRINE.md
  - trader-language-doctrine.md
authoritative_statement: >
  Missing-information states must preserve uncertainty while helping the
  operator understand what happened, why it matters, whether work can continue,
  and what to do next.
---

# Missing Information Doctrine

## Purpose

TradeForge exposes uncertainty rather than hiding it. Missing-information states
must therefore do more than report absence. They must support operator judgment.

## Core Principle

> Missing information is part of the decision context, not a dead-end UI state.

## Required Questions

Every operator-facing missing-information state should answer, when relevant:

1. What is missing?
2. Why is it missing?
3. Why does that matter here?
4. Can the operator continue?
5. What can the operator do next?

## Distinct Missing-State Classes

Do not collapse these into one generic `unavailable` message:

- not requested yet
- requested and loading
- provider failed
- provider not configured
- unsupported instrument or unsupported coverage
- intentionally omitted
- stale or expired context

Each class may imply a different next action and different workflow meaning.

## Operator-Facing Pattern

Prefer:

```text
Fundamentals have not been loaded yet.
You can continue technical evaluation, or load fundamentals before building conviction.
```

or:

```text
Fundamentals request failed from FMP; Alpha Vantage fallback was not available.
This does not block setup evaluation, but valuation context is incomplete.
```

Avoid:

```text
Fundamentals unavailable.
```

when the system knows more.

## Recovery Rule

If a recovery action exists, show it.

Examples:

- load
- retry
- choose provider
- inspect provider status
- continue without this context
- switch to a more relevant context family

If no recovery exists, say so directly rather than implying the operator missed
an available action.

## Continuity Rule

The state should explain whether the current workflow can continue:

- continue safely
- continue with caution
- pause because required context is missing
- switch context type because the current one is semantically mismatched

## Authority Rule

Missing advisory context does not become canonical truth and does not silently
change lifecycle state. It changes what is known, not what has happened.

## Related Follow-On Work

- `TF-F031`
- `TF-F032`
- `TF-F033`
- `TF-F034`

