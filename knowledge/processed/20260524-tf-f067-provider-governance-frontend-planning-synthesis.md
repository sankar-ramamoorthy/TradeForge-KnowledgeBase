---
title: TF-F067 Provider Governance Frontend Planning Synthesis
type: processed-synthesis
status: processed
created: 2026-05-24
source_raw:
  - knowledge/raw/20260524-tf-f067-provider-governance-frontend-planning.md
source_issue: TF-F067
milestone: M13A
tags:
  - TradeForge
  - M13A
  - provider-governance
  - frontend
---

# TF-F067 Provider Governance Frontend Planning Synthesis

TF-F067 moves provider administration out of contextual right rails and into a dedicated external-systems governance surface.

## Stabilized Design

The frontend should distinguish two surfaces:

- Provider Governance surface: long-form credential management, capability routing, AI gateway metadata, diagnostics, and authority boundaries.
- Contextual rail: compact status/provenance/fallback/health summary with a configure link.

Provider Governance may be reachable from navigation, but it must not be described as a canonical decision workspace or lifecycle stage.

## Boundary Preservation

The UI must not imply provider state owns lifecycle truth, event truth, or trade authority. It should display provider/gateway state as operational support for human judgment.

## ADR Result

No new ADR is required; this implements the M13A design record and existing frontend/runtime ADRs.
