---
title: Attention Continuity Pattern
status: processed
type: processed-knowledge
created: 2026-05-14
updated: 2026-05-14
source_issues:
  - TF-0064
tags:
  - attention
  - continuity
  - sidebar
  - cross-workspace
  - ux-architecture
  - workflow-centric
---

# Attention Continuity Pattern

## Purpose

Operational attention state must travel with the user across workspace transitions.
`AttentionSummaryPanel` makes the attention queue visible in the sidebar on every
workspace — not just the Operating Workspace.

## Architecture

### Placement

Sidebar, between `ActiveDecisionBadge` and `SessionPanel`. The sidebar persists
across all workspace navigations; it is the natural home for persistent state.

### Self-contained fetch

`AttentionSummaryPanel` fetches `GET /workspaces/operating/attention` independently.
It does not receive queue data from App.tsx. This keeps it decoupled from the
OperatingWorkspace's own queue fetch.

### Render conditions

- Returns null when `params.decision_id` is empty (no active decision)
- Returns null while loading (no loading spinner — silent)
- Renders "Queue clear ✓" when `items.length === 0`
- Renders count + urgency + top item + link when `items.length > 0`

### "Queue clear" as a feature

The explicit "Queue clear" state is intentional: without it, the panel disappears
when the queue is empty, creating ambiguity about whether attention state was lost.
An explicit clear indicator tells the user the system is aware and nothing is pending.

## Refetch triggers

`useEffect` deps: `[decision_id, persona_id, workspace_id]`

Refetches when:
- A new decision is activated (decision_id changes)
- The session context changes

Does NOT refetch:
- On workspace navigation (sidebar doesn't unmount — state persists across workspaces)
- On stage changes (attention state changes propagate via new decisions/actions)

## Related

- [[operational-context-pattern]] — watches symbols + stage persistence in localStorage
- [[stage-aware-navigation-pattern]] — nav indicator for recommended workspace
- TF-0022: OperationalAttentionQueueReadService (backend service)
- TF-0035: OperatingWorkspace (full queue display)
