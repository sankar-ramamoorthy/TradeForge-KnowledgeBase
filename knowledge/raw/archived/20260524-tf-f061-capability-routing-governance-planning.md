---
title: TF-F061 Capability Routing Governance Planning
type: planning-note
status: raw
created: 2026-05-24
tags: [tradeforge, tf-f061, m13a, capability-routing, provider-governance]
issue: TF-F061
milestone: M13A
---

# TF-F061 Capability Routing Governance Planning

## Issue Goal

Define the governance model for capability-first provider routing before API or
frontend implementation begins.

## Architecture Reasoning

M10D introduced provider identity and provider capability as separate concepts.
M13A needs to preserve that distinction while expanding governance around
configured providers, preferred providers, fallback routes, and selected
providers.

The default policy should remain deterministic primary plus ordered fallback
selection. Weighted routing, hidden provider quality scoring, cost optimization,
and workspace-scoped policies are deferred.

## Design Boundaries

The model should cover price snapshots, fundamentals, AI advisory, and future
broker/paper trading. It should show what must be operator-visible and how route
state relates to replay provenance without becoming canonical event-ledger
truth.

## Runtime Output

```text
DOCS/capability-routing-governance-model.md
```
