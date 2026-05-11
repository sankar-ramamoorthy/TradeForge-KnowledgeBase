---
title: ADR Index
type: index
status: canonical
tags: [TradeForge, ADR, architecture, index]
created: 2026-05-08
updated: 2026-05-11
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
- [ADR 0018: Postgres Event Store Persistence](../../../../TradeForge/DOCS/adr/0018-postgres-event-store-persistence.md)

Related concepts:

- [[Semantic Truth Layer]]
- [[Architectural Memory]]
- [[Event Ledger]]
- [[Event Store Port]]
- [[Decision Lifecycle Engine]]

Note: ADR 0011 and ADR 0018 are infrastructure-scoped. They define repeatable runtime environment and persistence boundaries without redefining domain semantics.

---

## Replay And Interpretation Layer

- [ADR 0008: Replay System Design](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)
- [ADR 0009: Persona Interpretation Model](../../../../TradeForge/DOCS/adr/0009-persona-interpretation-model.md)
- [ADR 0010: Market Intelligence Interpretation Layer](../../../../TradeForge/DOCS/adr/0010-market-intelligence-interpretation-layer.md)
- [ADR 0014: Replay-Centric UX Model](../../../../TradeForge/DOCS/adr/0014-replay-centric-ux-model.md)

Related concepts:

- [[Replay System]]
- [[Replay Session]]
- [[ReplayTimeline]]
- [[HistoricalReconstruction]]
- [[Persona]]
- [[Workspace]]
- [[Market Intelligence Layer]]
- [[Inferred State]]

---

## Workspace And MVP Architecture Layer

- [ADR 0012: Workspace Architecture Model](../../../../TradeForge/DOCS/adr/0012-workspace-architecture-model.md)
- [ADR 0013: Operational Attention Model](../../../../TradeForge/DOCS/adr/0013-operational-attention-model.md)
- [ADR 0023: MVP Vertical Slice Definition](../../../../TradeForge/DOCS/adr/0023-mvp-vertical-slice-definition.md)

Related concepts:

- [[Workspace]]
- [[Persona Workspace]]
- [[Decision Queue]]
- [[OperationalAttentionQueue|Operational Attention Queue]]
- [[WorkspaceSummary|Workspace Summary]]
- [[Replay Session]]
- [[Review Artifact]]

Note: ADR 0012 governs the TF-0014 workspace routing boundary. Routes are runtime entrypoints only; they do not own workspace truth, lifecycle state, or projection state.

---

## Numbering Note

The source raw note grouped ADRs by semantic layer but used shortened labels such as `ADR-001`. This index preserves the runtime repository's four-digit ADR numbering.
