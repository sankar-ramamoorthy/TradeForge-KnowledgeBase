---
title: ADR Index
type: index
status: canonical
tags: [TradeForge, ADR, architecture, index]
created: 2026-05-08
updated: 2026-05-08
---

# ADR Index

## Purpose

This index links the current runtime ADRs into the knowledge-base navigation layer.

Runtime ADRs remain implementation-alignment records in:

```text
../../../../TradeForge/DOCS/adr/
```

This index does not promote those ADRs into KB-native canonical ADRs by itself. It provides semantic navigation and layer grouping for progressive discovery.

---

## Foundational Truth Layer

- [ADR 0001: Event Sourcing Core Model](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md)
- [ADR 0003: Canonical Event Taxonomy](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)

Related concepts:

- [[Event Ledger]]
- [[Event]]
- [[Canonical State]]
- [[Derived State]]
- [[Replay System]]

---

## Workflow And Lifecycle Layer

- [ADR 0002: Decision Lifecycle Engine](../../../../TradeForge/DOCS/adr/0002-decision-lifecycle-engine.md)
- [ADR 0004: Workspace Projection Model](../../../../TradeForge/DOCS/adr/0004-workspace-projection-model.md)
- [ADR 0005: Scenario Engine Architecture](../../../../TradeForge/DOCS/adr/0005-scenario-engine-architecture.md)

Related concepts:

- [[Decision Lifecycle Engine]]
- [[Persona Workspace]]
- [[Workspace Surface]]
- [[Scenario Discovery Engine]]
- [[Decision Queue]]

---

## Governance Layer

- [ADR 0006: AI Advisory Boundary Model](../../../../TradeForge/DOCS/adr/0006-ai-advisory-boundary-model.md)
- [ADR 0007: Anti-Dashboard UX Decision](../../../../TradeForge/DOCS/adr/0007-anti-dashboard-ux-decision.md)

Related concepts:

- [[AI Advisory Boundary]]
- [[Human Decision Sovereignty]]
- [[UX_DOCTRINE]]
- [[Persona Workspace]]

---

## Runtime Environment Layer

- [ADR 0011: Runtime Development Environment](../../../../TradeForge/DOCS/adr/0011-runtime-development-environment.md)

Related concepts:

- [[Semantic Truth Layer]]
- [[Architectural Memory]]
- [[Event Ledger]]
- [[Decision Lifecycle Engine]]

Note: ADR 0011 is infrastructure-scoped. It defines repeatable runtime tooling and does not define domain semantics.

---

## Replay And Interpretation Layer

- [ADR 0008: Replay System Design](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)
- [ADR 0009: Persona Interpretation Model](../../../../TradeForge/DOCS/adr/0009-persona-interpretation-model.md)
- [ADR 0010: Market Intelligence Interpretation Layer](../../../../TradeForge/DOCS/adr/0010-market-intelligence-interpretation-layer.md)

Related concepts:

- [[Replay System]]
- [[Replay Session]]
- [[Persona]]
- [[Market Intelligence Layer]]
- [[Inferred State]]

---

## Numbering Note

The source raw note grouped ADRs by semantic layer but used shortened labels such as `ADR-001`. This index preserves the runtime repository's four-digit ADR numbering: `ADR 0001` through `ADR 0011`.
