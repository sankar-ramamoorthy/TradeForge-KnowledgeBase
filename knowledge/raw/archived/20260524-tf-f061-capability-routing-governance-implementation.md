---
title: TF-F061 Capability Routing Governance Implementation
type: implementation-note
status: raw
created: 2026-05-24
tags: [tradeforge, tf-f061, m13a, capability-routing]
issue: TF-F061
milestone: M13A
---

# TF-F061 Capability Routing Governance Implementation

## Implementation Summary

Added runtime design documentation for the M13A capability routing governance
model.

Runtime file added:

```text
DOCS/capability-routing-governance-model.md
```

Operational state updated:

- `TF-F061` marked Done in `DOCS/ISSUE_REGISTER.md`
- Roadmap M13A linked issue entry updated to show TF-F061 Done

## Stabilized Outcomes

The model defines provider identity, provider capability, configured provider,
preferred provider, fallback provider, selected provider, and capability route.
It preserves deterministic preferred-plus-fallback selection and defers more
complex routing policy.

## Invariant Review

Capability routing remains operational governance. It does not own lifecycle
state, approve plans, execute trades, or make external provider state canonical.
