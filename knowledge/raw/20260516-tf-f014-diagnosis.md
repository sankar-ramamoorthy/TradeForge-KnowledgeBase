---
title: TF-F014 Diagnosis - Remaining Workspace Zoning Propagation
type: raw-diagnosis
status: raw
tags: [TradeForge, TF-F014, feedback, frontend, workspace-layout]
created: 2026-05-16
updated: 2026-05-16
source_history:
  - knowledge/raw/2026-05-16 feed back on screen real estate and design.md
  - knowledge/processed/20260516-tf-f012-synthesis.md
related:
  - "[[UX_DOCTRINE]]"
  - "[[WORKSPACES]]"
issues:
  - TF-F014
---

# TF-F014 Diagnosis - Remaining Workspace Zoning Propagation

## Observable Gap

After the `TF-F012` reference implementation, the remaining market-context workspaces still use inline context composition:

- `OpportunityWorkspace` renders `MarketContextPanel` inline
- `ActivePositionWorkspace` renders `MarketContextPanel` inline

The three-zone workstation model now exists, but it is not yet propagated across the other workspaces that visibly need it.

## Root Cause

`TF-F012` was intentionally scoped to:

- shared layout primitive alignment
- one reference workspace conversion

The remaining gap is not a failure of the first fix. It is the next bounded propagation step.

## Minimum Correct Scope

- evaluate the remaining workspaces that contain persistent market/context content
- migrate appropriate contextual surfaces into the new rail model
- preserve workspace-specific differences rather than forcing identical layouts

## Explicitly Out Of Scope

- changing lifecycle semantics
- generic redesign of all workspace surfaces
- panel customization systems

## ADR Checkpoint

No ADR currently required.

## Diagnosis Conclusion

`TF-F014` should track follow-on workstation-zoning propagation after the `TF-F012` reference implementation.
