---
title: Demo Scenario Architecture
status: processed
type: processed-knowledge
created: 2026-05-14
updated: 2026-05-14
source_issues:
  - TF-0058
  - TF-0059
source_history:
  - knowledge/raw/20260513-tf0058-guided-demo-mode-planning.md
  - knowledge/raw/20260513-tf0058-guided-demo-mode-implementation.md
  - knowledge/raw/20260514-tf-0059-seeded-demo-scenarios-planning.md
  - knowledge/raw/20260514-tf-0059-seeded-demo-scenarios-implementation.md
tags:
  - demo
  - onboarding
  - demoability
  - lifecycle
  - replay
  - frontend
---

# Demo Scenario Architecture

## Pattern

TradeForge implements named demo scenarios as declarative data objects in `frontend/src/demo.ts`.

Each scenario is a `DemoScenario` value object specifying:
- `id`, `symbol`, `name`, `description` — identity and presentation
- `targetStage` — how deep to seed the lifecycle (Plan / Approval / Position / Review)
- `landingPath` — which workspace to navigate to after seeding
- Stage-specific payloads (`initial_thesis`, `plan_notes`, `approval_notes`, etc.)

## Seeding Model

`runDemoFlow(scenario, options)` executes sequential lifecycle API calls through the same surface as normal user workflow. Demo transitions carry `provenance: { actor: "demo", source: "guided-demo-mode" }`.

Seeding depth corresponds to lifecycle event count:
- Plan: 3 events (init + Thesis + Plan)
- Position: 6 events
- Review: 7 events (full lifecycle)

## Replay Enablement

Scenarios seeded to Review stage produce a complete 7-stage event timeline visible in the Replay Workspace. This is the canonical mechanism for making the Replay Workspace contain meaningful content before a user has traversed the lifecycle manually.

## Invariant Compliance

Demo scenarios do NOT bypass lifecycle rules — they use the same API surface as human workflow. Human decision sovereignty is preserved because the demo uses advisory provenance and does not grant lifecycle authority.

## Extensibility

Adding new scenarios requires only appending to the `DEMO_SCENARIOS` array. The UI, navigation, and seeding logic are scenario-agnostic.

## Landing Path Convention

Each scenario has a `landingPath` matching its natural entry point:
- Plan-stage scenarios → Plan Review workspace
- Position-stage scenarios → Active Position workspace
- Review-stage scenarios → Replay or Review workspace

## Active Decision Badge

The `ActiveDecisionRecord` carries `scenario_name?: string` for display in the sidebar badge, surfacing which named scenario is active without affecting projection or lifecycle logic.
