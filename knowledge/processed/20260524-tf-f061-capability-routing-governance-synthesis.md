---
title: TF-F061 Capability Routing Governance Synthesis
type: processed-synthesis
status: processed
created: 2026-05-24
source_history:
  - knowledge/raw/archived/20260524-tf-f061-capability-routing-governance-planning.md
  - knowledge/raw/archived/20260524-tf-f061-capability-routing-governance-implementation.md
related_runtime:
  - DOCS/capability-routing-governance-model.md
issue: TF-F061
milestone: M13A
tags:
  - capability-routing
  - provider-governance
  - m13a
---

# TF-F061 Capability Routing Governance Synthesis

## Stabilized Knowledge

M13A preserves capability-first provider governance. TradeForge should reason in
capabilities and operational roles before raw provider names.

The default route policy is deterministic:

```text
selected_provider = first usable provider from [preferred, ...fallbacks]
```

This preserves the M10D provider capability architecture while preparing the
runtime for explicit governance surfaces.

## Runtime Design Outcome

`DOCS/capability-routing-governance-model.md` defines provider identity,
provider capability, configured provider, preferred provider, fallback provider,
selected provider, and capability route as separate concepts.

## Replay And Provenance

Capability route state can be replay-relevant because it affects which external
context was visible to the operator. It does not become canonical event-ledger
truth by default. Replay must use captured historical provenance or explicitly
show route context as unavailable rather than calling live providers.

## Deferred Work

Weighted routing, workspace-scoped policies, persona-scoped provider
preferences, cost-aware routing, adaptive provider scoring, and broker execution
routing are deferred beyond TF-F061.

## Operational Sync

`TF-F061` is complete in the runtime issue register and marked complete in the
M13A roadmap issue list. TF-F062 is the next M13A issue.
