---
title: TF-F060 Provider Governance Control Surface Planning
type: planning-note
status: raw
created: 2026-05-24
tags: [tradeforge, tf-f060, m13a, provider-governance, operational-ux, control-surface]
issue: TF-F060
milestone: M13A
---

# TF-F060 Provider Governance Control Surface Planning

## Issue Goal

Define the provider governance control surface before implementation begins.
The surface must separate external-systems governance from contextual decision
workflow rails.

## Architecture Reasoning

The processed M13A synthesis stabilized the core distinction:

```text
Credential != Provider != Capability != Model
```

The control surface must keep these concepts separate while preserving the
TradeForge workspace doctrine. Provider governance may have a runtime route or
page, but it is not a canonical Persona Workspace because it does not own
persona cognition, lifecycle progression, exposure supervision, opportunity
evaluation, or review continuity.

## Design Boundaries

The design should specify:

- Overview
- Credentials
- Market Data Providers
- Broker Providers
- AI Gateway
- Capability Routing
- Diagnostics

Contextual rails should show status, provenance, freshness, fallback, advisory
boundary, and a configure link. They should not host credential entry, model
selection, route management, or diagnostics administration.

## Event And Replay Considerations

TF-F060 is design-only and introduces no event model changes. Provider and
gateway state may be replay-relevant later, but provider health or route state
must not become canonical event-ledger truth by implication.

## Implementation Boundary

Runtime implementation for TF-F060 is a design document only:

```text
DOCS/provider-governance-control-surface.md
```
