---
title: Issue Register
type: index
status: canonical
tags: [TradeForge, issues, architecture, roadmap]
created: 2026-05-08
updated: 2026-05-08
---

# Issue Register

## Purpose

This document is the knowledge-base issue register for semantic and architectural concerns.

It mirrors runtime issue IDs so architectural intent can be traced to implementation work, but it is not an execution tracker.

Authority split:

- Knowledge-base issue register = semantic and architectural issue inventory
- Runtime issue register = implementation planning and local coding discipline
- GitHub issues = execution tracking

Runtime implementation issue tracking lives in:

```text
../../../../TradeForge/DOCS/ISSUE_REGISTER.md
```

---

## Register Rules

- Keep runtime issue IDs for traceability.
- Use `KB-*` issue IDs for semantic architecture work that does not map directly to a runtime implementation issue.
- Capture semantic concern before implementation detail.
- Link ADRs, canonical concepts, and workflows where available.
- Do not use this file for branch naming, scheduling, or task ownership.
- Surface contradictions with doctrine explicitly rather than silently rewriting meaning.

---

## Issue Index

| ID | Status | Milestone | Semantic Concern |
| --- | --- | --- | --- |
| KB-0001 | Done | M1A | Canonical entity semantics |
| TF-0001 | Done | M0 | Planning discipline and architectural memory |
| TF-0002 | Done | M1 | Python project scaffold must not define domain semantics |
| TF-0003 | Done | M1 | Dockerfile must remain infrastructure-scoped |
| TF-0004 | Done | M1 | Docker Compose must not imply distributed architecture |
| TF-0005 | Done | M1 | Test baseline must support domain verification without external services |
| TF-0006 | Planned | M1 | Dev command conventions must stay separate from semantic authority |
| TF-0007 | Planned | M1 | README setup must point back to ADR and issue discipline |
| TF-0008 | Planned | M2 | Event envelope and canonical event domains |
| TF-0009 | Planned | M2 | Append-only Event Ledger interface |
| TF-0010 | Planned | M2 | Early event store adapter without semantic drift |
| TF-0011 | Planned | M3 | Canonical lifecycle state model |
| TF-0012 | Planned | M3 | Deterministic lifecycle transition validation |
| TF-0013 | Planned | M3 | Lifecycle orchestration without state ownership drift |
| TF-0014 | Planned | M4 | Replay projector foundation |
| TF-0015 | Planned | M4/M5 | Persona Workspace projection read model |
| TF-0016 | Planned | M5 | Persona context model |
| TF-0017 | Planned | M6 | Market Intelligence interpretation model |
| TF-0018 | Planned | M6 | Scenario Discovery advisory model |
| TF-0019 | Planned | M7 | AI advisory boundary interfaces |
| TF-0020 | Planned | M8 | First replayable Idea-through-Review vertical slice |

---

## KB-0001: Define Canonical Entity Semantics

**Status:** Done

**Milestone:** M1A

**Semantic Concern:** TradeForge has architectural doctrine and runtime ADR alignment, and now has initial canonical semantic objects defined as durable entity pages.

**Architectural Concern:** Event-sourced systems depend on precise entity semantics. Entity ambiguity causes downstream ambiguity in event schemas, lifecycle rules, projections, and replay.

**Initial Entity Backlog:**

- [[TradeIdea]]
- [[TradeThesis]]
- [[TradePlan]]
- [[Scenario]]
- [[Workspace]]
- [[LifecycleEvent]]
- [[Persona]]
- [[ReplaySession]]
- [[ReviewArtifact]]
- [[MarketContext]]
- [[PositionIntent]]
- [[ExecutionIntent]]

**Linked ADRs:**

- [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md)
- [ADR 0002](../../../../TradeForge/DOCS/adr/0002-decision-lifecycle-engine.md)
- [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)
- [ADR 0004](../../../../TradeForge/DOCS/adr/0004-workspace-projection-model.md)
- [ADR 0005](../../../../TradeForge/DOCS/adr/0005-scenario-engine-architecture.md)
- [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)
- [ADR 0010](../../../../TradeForge/DOCS/adr/0010-market-intelligence-interpretation-layer.md)

**KB Acceptance Meaning:**

- Canonical entity definition work is explicitly tracked as KB semantic architecture work.
- Entity definitions are distinguished from runtime model implementation.
- Entity pages define meaning, lifecycle role, event relationships, replay relevance, and authority boundaries.
- Runtime models may implement these concepts, but they do not become the source of semantic truth.

**Completion Note:**

Initial canonical entity pages exist under `knowledge/entities/`.

---

## TF-0001: Establish Milestone Roadmap And Issue Register

**Status:** Done

**Milestone:** M0

**Semantic Concern:** TradeForge needs durable separation between semantic truth, runtime planning, and execution tracking.

**Linked ADRs:** ADR 0001 through ADR 0011.

**KB Acceptance Meaning:** Runtime planning discipline exists without making implementation tasks semantic authority.

---

## TF-0002: Create Python Project Scaffold With pyproject.toml And uv

**Status:** Done

**Milestone:** M1

**Semantic Concern:** Runtime project metadata and `uv` workflow must support implementation without defining domain meaning.

**Linked ADRs:** [ADR 0011](../../../../TradeForge/DOCS/adr/0011-runtime-development-environment.md)

**KB Acceptance Meaning:** Python 3.12 and `uv` are environment conventions, not architectural semantics.

**Completion Note:**

Runtime `TF-0002` added `pyproject.toml` and `uv.lock`. Verification included `uv --version`, `uv lock`, and `uv sync` with CPython 3.12. No Docker, pytest, domain model, event model, or service code was added.

---

## TF-0003: Add Dockerfile Using uv Python 3.12 Slim Base Image

**Status:** Done

**Milestone:** M1

**Semantic Concern:** Docker must remain an execution environment concern.

**Linked ADRs:** [ADR 0011](../../../../TradeForge/DOCS/adr/0011-runtime-development-environment.md)

**KB Acceptance Meaning:** Container configuration does not encode lifecycle, event, workspace, persona, scenario, replay, or AI authority rules.

**Completion Note:**

Runtime `TF-0003` added `Dockerfile` and `.dockerignore`. Verification included `docker build -t tradeforge-runtime:tf-0003 .` and `docker run --rm tradeforge-runtime:tf-0003`, which printed `Python 3.12.12`. No Compose, pytest, domain model, service code, broker, or database behavior was added.

---

## TF-0004: Add docker-compose.yml For Local Development

**Status:** Done

**Milestone:** M1

**Semantic Concern:** Docker Compose must not imply microservice architecture or distributed system semantics.

**Linked ADRs:** [ADR 0011](../../../../TradeForge/DOCS/adr/0011-runtime-development-environment.md)

**KB Acceptance Meaning:** Compose is a local development entrypoint, not a boundary model.

**Completion Note:**

Runtime `TF-0004` added `docker-compose.yml` with one local `tradeforge` service, local Dockerfile build, repo bind mount, and named `/app/.venv` volume so container `uv` does not alter the host venv. Verification included `docker compose config` and `docker compose run --rm tradeforge`, which printed `Python 3.12.12`.

---

## TF-0005: Add Pytest Baseline And Test Command

**Status:** Done

**Milestone:** M1

**Semantic Concern:** The test baseline must make future domain behavior verifiable without depending on live external systems.

**Linked ADRs:** [ADR 0011](../../../../TradeForge/DOCS/adr/0011-runtime-development-environment.md)

**KB Acceptance Meaning:** Test tooling supports replayable, deterministic implementation work.

**Completion Note:**

Runtime `TF-0005` added pytest dev dependency/config in `pyproject.toml` and a baseline Python 3.12 scaffold test in `tests/test_scaffold.py`. Verification included `uv run pytest`, which passed with `1 passed`.

---

## TF-0006: Add Lint, Type, And Dev Command Conventions

**Status:** Planned

**Milestone:** M1

**Semantic Concern:** Development command conventions should improve consistency without becoming semantic authority.

**Linked ADRs:** [ADR 0011](../../../../TradeForge/DOCS/adr/0011-runtime-development-environment.md)

**KB Acceptance Meaning:** Tool commands remain subordinate to doctrine, ADRs, and canonical entity definitions.

---

## TF-0007: Add README Developer Setup Section

**Status:** Planned

**Milestone:** M1

**Semantic Concern:** Developer setup documentation should reinforce the distinction between tooling and domain architecture.

**Linked ADRs:** [ADR 0011](../../../../TradeForge/DOCS/adr/0011-runtime-development-environment.md)

**KB Acceptance Meaning:** Runtime README setup should point implementers back to ADR and issue discipline before code changes.

---

## TF-0008: Define Event Envelope And Canonical Event Domains

**Status:** Planned

**Milestone:** M2

**Semantic Concern:** Events must be modeled as immutable facts with stable domain categories.

**Linked ADRs:** [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md), [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md), [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)

**Linked Concepts:** [[Event Ledger]], [[LifecycleEvent]], [[Canonical State]], [[Derived State]], [[Inferred State]]

**KB Acceptance Meaning:** Event meaning is not defined by persistence infrastructure or runtime scaffolding.

---

## TF-0009: Define Append-Only Event Store Interface

**Status:** Planned

**Milestone:** M2

**Semantic Concern:** The Event Ledger must expose append-only truth semantics.

**Linked ADRs:** [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md), [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md), [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)

**Linked Concepts:** [[Event Ledger]], [[ReplaySession]], [[Canonical State]]

**KB Acceptance Meaning:** Interfaces must not normalize historical mutation, deletion, or projection-authored truth.

---

## TF-0010: Implement In-Memory Event Store Adapter

**Status:** Planned

**Milestone:** M2

**Semantic Concern:** Early adapters must not weaken Event Ledger meaning.

**Linked ADRs:** [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md), [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md), [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)

**KB Acceptance Meaning:** Adapter behavior preserves append-only assumptions and deterministic replay reads.

---

## TF-0011: Define Lifecycle State Model

**Status:** Planned

**Milestone:** M3

**Semantic Concern:** The decision lifecycle must remain explicit and stageful.

**Linked ADRs:** [ADR 0002](../../../../TradeForge/DOCS/adr/0002-decision-lifecycle-engine.md), [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)

**Linked Concepts:** [[TradeIdea]], [[TradeThesis]], [[TradePlan]], [[PositionIntent]], [[ExecutionIntent]], [[ReviewArtifact]]

**KB Acceptance Meaning:** Lifecycle stages remain `Idea`, `Thesis`, `Plan`, `Approval`, `Execution`, `Position`, and `Review`.

---

## TF-0012: Implement Lifecycle Transition Validator

**Status:** Planned

**Milestone:** M3

**Semantic Concern:** Decision progression must be governed by deterministic workflow rules.

**Linked ADRs:** [ADR 0002](../../../../TradeForge/DOCS/adr/0002-decision-lifecycle-engine.md), [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)

**KB Acceptance Meaning:** Invalid shortcuts do not create canonical facts.

---

## TF-0013: Implement Lifecycle Orchestration Service

**Status:** Planned

**Milestone:** M3

**Semantic Concern:** Runtime services may coordinate lifecycle work, but they must not redefine lifecycle authority.

**Linked ADRs:** [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md), [ADR 0002](../../../../TradeForge/DOCS/adr/0002-decision-lifecycle-engine.md), [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md)

**KB Acceptance Meaning:** Services orchestrate requests without becoming workflow truth.

---

## TF-0014: Implement Replay Projector Foundation

**Status:** Planned

**Milestone:** M4

**Semantic Concern:** Replay must reconstruct historical system state from durable facts.

**Linked ADRs:** [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md), [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md), [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)

**Linked Concepts:** [[ReplaySession]], [[LifecycleEvent]], [[Event Ledger]]

**KB Acceptance Meaning:** Replay output is deterministic for the same event stream and does not depend on live APIs.

---

## TF-0015: Implement Workspace Projection Read Model

**Status:** Planned

**Milestone:** M4/M5

**Semantic Concern:** Persona Workspaces need derived surfaces that support cognition without becoming state owners.

**Linked ADRs:** [ADR 0004](../../../../TradeForge/DOCS/adr/0004-workspace-projection-model.md), [ADR 0007](../../../../TradeForge/DOCS/adr/0007-anti-dashboard-ux-decision.md), [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md), [ADR 0009](../../../../TradeForge/DOCS/adr/0009-persona-interpretation-model.md)

**Linked Concepts:** [[Workspace]], [[Persona]], [[ReplaySession]]

**KB Acceptance Meaning:** Workspace surfaces are derived views, not canonical state.

---

## TF-0016: Define Persona Context Model

**Status:** Planned

**Milestone:** M5

**Semantic Concern:** Personas shape interpretation and prioritization without becoming users, profiles, or state authorities.

**Linked ADRs:** [ADR 0004](../../../../TradeForge/DOCS/adr/0004-workspace-projection-model.md), [ADR 0009](../../../../TradeForge/DOCS/adr/0009-persona-interpretation-model.md)

**Linked Concepts:** [[Persona]], [[Workspace]]

**KB Acceptance Meaning:** Persona influence remains interpretive and replay-compatible.

---

## TF-0017: Define Market Intelligence Interpretation Model

**Status:** Planned

**Milestone:** M6

**Semantic Concern:** Market Intelligence must provide interpreted context rather than decisions.

**Linked ADRs:** [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md), [ADR 0009](../../../../TradeForge/DOCS/adr/0009-persona-interpretation-model.md), [ADR 0010](../../../../TradeForge/DOCS/adr/0010-market-intelligence-interpretation-layer.md)

**Linked Concepts:** [[MarketContext]], [[Persona]], [[Scenario]]

**KB Acceptance Meaning:** Market observations remain distinguishable from interpretations.

---

## TF-0018: Define Scenario Discovery Advisory Model

**Status:** Planned

**Milestone:** M6

**Semantic Concern:** Scenarios are structured hypotheses, not trade decisions.

**Linked ADRs:** [ADR 0005](../../../../TradeForge/DOCS/adr/0005-scenario-engine-architecture.md), [ADR 0009](../../../../TradeForge/DOCS/adr/0009-persona-interpretation-model.md), [ADR 0010](../../../../TradeForge/DOCS/adr/0010-market-intelligence-interpretation-layer.md)

**Linked Concepts:** [[Scenario]], [[MarketContext]], [[TradeIdea]]

**KB Acceptance Meaning:** Scenario outputs do not become trades, positions, or execution instructions.

---

## TF-0019: Define AI Advisory Boundary Interfaces

**Status:** Planned

**Milestone:** M7

**Semantic Concern:** AI must remain advisory and non-authoritative.

**Linked ADRs:** [ADR 0006](../../../../TradeForge/DOCS/adr/0006-ai-advisory-boundary-model.md), [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md), [ADR 0010](../../../../TradeForge/DOCS/adr/0010-market-intelligence-interpretation-layer.md)

**KB Acceptance Meaning:** AI cannot directly append canonical decision or execution events, approve plans, or transition lifecycle state.

---

## TF-0020: Implement First Replayable Idea-Through-Review Vertical Slice

**Status:** Planned

**Milestone:** M8

**Semantic Concern:** The first vertical slice must demonstrate TradeForge as a replayable decision-support system, not a CRUD tracker or autonomous trading loop.

**Linked ADRs:** [ADR 0001](../../../../TradeForge/DOCS/adr/0001-event-sourcing-core-model.md), [ADR 0002](../../../../TradeForge/DOCS/adr/0002-decision-lifecycle-engine.md), [ADR 0003](../../../../TradeForge/DOCS/adr/0003-canonical-event-taxonomy.md), [ADR 0004](../../../../TradeForge/DOCS/adr/0004-workspace-projection-model.md), [ADR 0008](../../../../TradeForge/DOCS/adr/0008-replay-system-design.md)

**Linked Concepts:** [[TradeIdea]], [[TradeThesis]], [[TradePlan]], [[Workspace]], [[ReplaySession]], [[ReviewArtifact]]

**KB Acceptance Meaning:** The slice connects lifecycle, Event Ledger, workspace projection, replay, and review without live broker execution or autonomous AI control.
