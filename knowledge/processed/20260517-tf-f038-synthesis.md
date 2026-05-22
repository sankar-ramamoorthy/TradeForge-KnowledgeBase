---
title: TF-F038 Synthesis - Context Workbench Workspace Concept
type: processed-synthesis
status: processed
tags: [TradeForge, TF-F, M10E, context-workbench, workspace]
created: 2026-05-17
updated: 2026-05-17
source_history:
  - knowledge/raw/brainstorm-20260517-opportunity-workspace-ux-and-context-acquisition.md
  - knowledge/raw/20260517-tf-f028-to-tf-f042-diagnosis.md
  - knowledge/raw/20260517-tf-f038-planning.md
related:
  - "[[Workspaces Are Operational Environments]]"
issues:
  - TF-F038
---

# TF-F038 Synthesis - Context Workbench Workspace Concept

## What Was Stabilized

TradeForge now accepts the Context Workbench as the dedicated research-oriented
workspace concept for advisory context acquisition and interpretation.

## What Changed

- Added runtime `ADR-0040`.
- Added canonical `design/context-workbench.md`.

## Architectural Meaning

Opportunity evaluation and research acquisition are now explicitly distinct
workspace responsibilities. The Context Workbench owns acquisition, retrieval
state, provider transparency, and interpretation before later handoff into
opportunity or thesis work.

