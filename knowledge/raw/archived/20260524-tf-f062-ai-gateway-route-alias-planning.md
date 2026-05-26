---
title: TF-F062 AI Gateway Route Alias Planning
type: planning-note
status: raw
created: 2026-05-24
tags: [tradeforge, tf-f062, m13a, ai-gateway, litellm, route-aliases]
issue: TF-F062
milestone: M13A
---

# TF-F062 AI Gateway Route Alias Planning

## Issue Goal

Define LiteLLM as an AI gateway and route boundary rather than an ordinary
single-provider integration.

## Architecture Reasoning

M13A distinguishes Credential, Provider, Capability, and Model. LiteLLM changes
the provider model because one configured gateway can route to many underlying
model providers. TradeForge should request semantic advisory roles or route
aliases rather than raw model names throughout workflow logic.

## Design Boundaries

The design should define route aliases such as `tf-fast`, `tf-reasoning`,
`tf-long-context`, `tf-cheap`, and `tf-local`. It should define gateway
visibility, credential boundaries, replay/provenance expectations, and authority
limits.

## Runtime Output

```text
DOCS/ai-gateway-route-alias-model.md
```
