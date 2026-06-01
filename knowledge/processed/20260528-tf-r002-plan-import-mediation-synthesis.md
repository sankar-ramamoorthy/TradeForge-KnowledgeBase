---
title: TF-R002 Plan Import Mediation Synthesis
type: processed-synthesis
status: processed-draft
created: 2026-05-28
tags:
  - TradeForge
  - TF-R002
  - M14C
  - plan-import
  - advisory-boundary
  - selective-promotion
---

# TF-R002 Plan Import Mediation Synthesis

TF-R002 confirms that the TF-R001 import-preview pattern can extend from Thesis
authoring to Plan authoring if execution-boundary controls become stricter.

The stable distinction is:

```text
advisory plan source
    -> mediated plan draft fields
    -> operator edit/confirm
    -> normal decision.plan_created event
```

Plan import mediation is not plan import authority.

Durable rules from the implementation:

- plan imports may mediate entry, stop, target, and risk-note rationale
- plan imports must not auto-populate sizing
- plan imports must not populate prices, quantities, broker order details, or
  execution instructions
- plan imports must not approve, arm, or execute plans
- field acceptance before submission remains non-canonical draft state
- provenance belongs on the operator-authored lifecycle event when the plan is
  manually created
- replay labels imported plan context as advisory source material, not
  execution authority

ADR consideration:

An ADR is not required for TF-R002 alone. If the same selective mediation model
is reused for Review imports or other lifecycle artifacts, an ADR should
formalize the cross-workspace import mediation pattern and its prohibited
authority boundaries.
