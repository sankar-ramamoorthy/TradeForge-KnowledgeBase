---
title: M10A Sub-Issues Synthesis — Structured Cognition Artifacts (AIS03–AIS12)
type: processed-synthesis
status: processed
milestone: M10A
created: 2026-05-14
source_history:
  - knowledge/raw/20260514-m10a-m10ais03-thesis-revision-planning.md
  - knowledge/raw/20260514-m10a-m10ais03-implemented.md
  - knowledge/raw/20260514-m10a-m10ais04-m10ais05-planning.md
  - knowledge/raw/20260514-m10a-m10ais06-m10ais07-planning.md
  - knowledge/raw/20260514-m10a-m10ais08-planning.md
  - knowledge/raw/20260514-m10a-m10ais09-planning.md
  - knowledge/raw/20260514-m10a-m10ais10-planning.md
  - knowledge/raw/20260514-m10a-m10ais11-m10ais12-planning.md
tags: [M10A, structured-cognition, thesis-revision, scenario-branch, trade-plan, review-reflection, replay]
related:
  - "[[Decision Lifecycle]]"
  - "[[Replayability Is Foundational]]"
  - "[[Human Decision Sovereignty]]"
---

# M10A Sub-Issues Synthesis — Structured Cognition Artifacts (AIS03–AIS12)

## Overview

M10A AIS03–AIS12 implement the structured cognitive artifact layer of the TradeForge decision lifecycle. Each artifact captures a distinct reasoning dimension without advancing the lifecycle stage — they are enrichment events appended directly to the event store.

## AIS03 — Thesis Revision History

Thesis revision is NOT a lifecycle transition — the stage remains Thesis. New event type `decision.thesis_revised` captures how operator reasoning evolved. `GET /lifecycle/decisions/{id}/thesis` scans both `thesis_created` and `thesis_revised` events, returning the most recent. `GET /lifecycle/decisions/{id}/thesis/history` returns all snapshots in chronological order. Revision is only valid at Thesis stage (before Plan is created).

## AIS04–05 — Scenario Branch Modeling

`ScenarioBranchArtifact` captures conditional reasoning: primary, alternative, invalidation, and regime-transition branches. New event type `decision.scenario_branch_created`. Branches are valid at any active stage (Idea through Position, not after Review). Frontend: `ScenarioBranchModal` + `ScenarioBranchPanel` in OpportunityWorkspace; `ReplayWorkspace` shows branch payloads inline in timeline.

## AIS06–07 — Structured Trade Plan

`TradePlanArtifact` gates the Thesis→Plan lifecycle transition. Fields: entry_rationale, stop_rationale, target_rationale, sizing_rationale, execution_assumptions, playbook_alignment. Follows the same pattern as AIS01-02 (thesis authoring).

## AIS08 — Plan Validation Preview

Backend endpoint surfaces cognition readiness and lifecycle rule previews before the operator authorizes a plan. Backend chosen over frontend-only logic because validation must be deterministic and replayable.

## AIS09 — Replay Cognitive Artifact Timeline

Replay surfaces thesis and plan cognitive artifact content inline within the event timeline so operators can reconstruct the operator's thinking at each lifecycle stage. No backend changes required — existing event payloads contain the content.

## AIS10 — Cognitive Snapshot Reconstruction

Given timestamp T: return the lifecycle stage, most recent thesis, most recent plan, and all scenario branches as they existed at time T. Distinct from AIS09 (which shows individual event payloads). AIS10 reconstructs the complete cognitive state at a point in time.

## AIS11–12 — Structured Review Reflection

`ReviewReflectionArtifact` closes the decision lifecycle. Separates decision_quality (was the reasoning sound?) from outcome_quality (did the trade work?). Frontend: `ReviewReflectionModal` + cognition-aware `ReviewWorkspace` update.

## Shared Invariant

All cognitive artifacts are enrichment events. They do not advance lifecycle state. They append directly to the event store (not via LifecycleOrchestrationService). They do not appear in LIFECYCLE_EVENT_STAGE_MAP.
