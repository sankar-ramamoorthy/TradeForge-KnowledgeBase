---
title: Milestone Roadmap Index
type: index
status: canonical
tags: [TradeForge, roadmap, milestones, architecture, MVP]
created: 2026-05-08
updated: 2026-05-10
runtime_authority: ../../../../TradeForge/DOCS/Milestone_Roadmap_v2.md
---

# Milestone Roadmap Index

## Purpose

This document is the KB semantic index for the active TradeForge roadmap.

It is not a line-for-line mirror of the runtime roadmap. The runtime implementation planning authority is:

```text
../../../../TradeForge/DOCS/Milestone_Roadmap_v2.md
../../../../TradeForge/DOCS/ISSUE_REGISTER.md
```

This KB index records semantic intent, doctrine alignment, and current milestone meaning so future agents can understand why the roadmap is structured this way.

## Active Roadmap

Active runtime roadmap:

```text
TradeForge/DOCS/Milestone_Roadmap_v2.md
```

The old runtime roadmap file was renamed to:

```text
TradeForge/DOCS/MILESTONE_ROADMAP_DEPRECATED.md
```

It is retained only as historical planning context.

Roadmap v2 supersedes the old backend-first roadmap direction.

Core shift:

```text
backend-semantic sequencing
-> operational cognition architecture sequencing
```

## Roadmap v2 Principle

TradeForge MVP v1 should prove:

```text
replayable discretionary trading cognition
```

It should not prove autonomous trading, broker replacement, charting platform sophistication, AI orchestration, simulation, or RL.

## Milestone Summary

| Milestone | Status | Semantic Meaning |
|---|---|---|
| M0 | Done | Planning discipline and architectural memory |
| M1 | Done | Runtime scaffold and development environment |
| M1A | Done | Canonical entity definitions |
| M2 | Done | Event Ledger and canonical event model |
| M3 | Done | Decision Lifecycle Engine |
| M4 | Done | Workspace and cognitive architecture |
| M5 | In Progress | Replay and projection foundation |
| M6 | Planned | Persona Workspace projection layer |
| M7 | Planned | MVP runtime infrastructure |
| M8 | Planned | First operational MVP vertical slice |
| M9+ | Deferred | Market/scenario/AI/adaptive layers after MVP path checkpoint |

## M4 Closeout

M4 is complete:

- TF-0014: Create workspace routing model (**Done**)
- TF-0015: Define workspace state contracts (**Done**)

TF-0014 established bounded runtime entrypoints for the ADR 0012 workspace set without making routes into workspace truth.

TF-0015 defined workspace state contracts while preserving the distinction between:

- canonical event-backed state
- derived workspace state
- inferred/advisory context
- route entry context

M5 is now in progress:

- TF-0016: Implement replay projector foundation (**Done**)
- TF-0017: Implement projection rebuild pipeline (**Done**)
- TF-0018: Implement replay timeline engine (**Planned**)
- TF-0019: Implement historical reconstruction pipeline (**Planned**)

TF-0016 established deterministic replay projection over ordered event history. It did not introduce projection persistence, replay timelines, historical reconstruction, replay APIs, AI narration, or frontend replay workspace behavior.

TF-0017 established deterministic projection rebuild orchestration. It did not introduce projection persistence, rebuild observability events, replay timelines, historical reconstruction, or API exposure.

## Fast MVP Path

```text
M4: lean workspace architecture
M5: replay/projection foundation
M6: persona workspace projections
M7: Postgres + API + React runtime infrastructure
M8: usable operational MVP workflow
```

## M4 Boundary Result

M4 did not include React implementation, Postgres, full workspace UI, Market Intelligence automation, Scenario Discovery, AI advisory, behavioral scoring, simulation, or RL.

Those remain outside the completed M4 boundary unless a later runtime issue or ADR explicitly pulls them forward.

## Runtime ADR Alignment

- [ADR 0012: Workspace Architecture Model](../../../../TradeForge/DOCS/adr/0012-workspace-architecture-model.md)
- [ADR 0013: Operational Attention Model](../../../../TradeForge/DOCS/adr/0013-operational-attention-model.md)
- [ADR 0014: Replay-Centric UX Model](../../../../TradeForge/DOCS/adr/0014-replay-centric-ux-model.md)
- [ADR 0023: MVP Vertical Slice Definition](../../../../TradeForge/DOCS/adr/0023-mvp-vertical-slice-definition.md)

## KB Alignment

Relevant KB artifacts:

- [[Design Directory Introduction Decision]]
- [[MVP v1 Workspace Architecture Synthesis]]
- [[MVP v1 Workspace Candidate Synthesis]]
- [[MVP v1 Mockup Design Observations]]
- [[Roadmap v2 Alignment And Processing Notes]]
- [[TF-0014 Workspace Routing Model Planning Synthesis]]
- [[Implemented TF-0014 Workspace Routing Model]]
- [[TF-0016 Replay Projector Foundation]]
- [[Implemented TF-0016 Replay Projector Foundation]]
- [[TF-0017 Projection Rebuild Pipeline]]
- [[Implemented TF-0017 Projection Rebuild Pipeline]]

Root design layer:

```text
design/
```

Status:

```text
draft design architecture, not canonical DESIGN.md
```

## Synchronization Rule

If runtime roadmap and this KB index diverge, treat the runtime roadmap as operational planning authority and update this KB index as a semantic summary.
