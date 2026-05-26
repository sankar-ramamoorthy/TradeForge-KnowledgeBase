---
title: TF-F060 Provider Governance Control Surface Synthesis
type: processed-synthesis
status: processed
created: 2026-05-24
source_history:
  - knowledge/raw/archived/20260524-tf-f060-provider-governance-control-surface-planning.md
  - knowledge/raw/archived/20260524-tf-f060-provider-governance-control-surface-implementation.md
related_runtime:
  - DOCS/provider-governance-control-surface.md
issue: TF-F060
milestone: M13A
tags:
  - provider-governance
  - control-surface
  - operational-ux
  - m13a
---

# TF-F060 Provider Governance Control Surface Synthesis

## Stabilized Knowledge

Provider governance is an external-systems control surface, not a canonical
TradeForge workspace. Runtime may expose it through a page or route, but KB
workspace doctrine remains unchanged: workspaces are persona-scoped decision
environments.

The stable M13A design distinction remains:

```text
Credential != Provider != Capability != Model
```

The control surface keeps these concepts separate while showing operator-visible
status, provenance, health, and routing information.

## Runtime Design Outcome

`DOCS/provider-governance-control-surface.md` defines the first M13A design
surface. It specifies modules for Overview, Credentials, Market Data Providers,
Broker Providers, AI Gateway, Capability Routing, and Diagnostics.

## Contextual Rail Rule

Contextual rails should be informational and provenance-oriented. They may show
capability, selected provider, fallback, health, freshness, advisory boundary,
and a configure link. They should not host long-form credential entry, route
management, model selection, diagnostics administration, or provider governance
policy editing.

## Ontology And ADR Implications

No new canonical entity was promoted. Candidate terms such as Provider
Governance, AI Gateway, Provider Health, and AI Route Alias remain M13A design
concepts until recurrence and implementation stabilize them further.

No ADR was required for TF-F060 because the issue documents an accepted design
surface without changing persistence authority, event taxonomy, replay behavior,
or integration boundary semantics.

## Operational Sync

`TF-F060` is complete in the runtime issue register and marked complete in the
M13A roadmap issue list. Implementation issues TF-F061 through TF-F068 remain
planned.
