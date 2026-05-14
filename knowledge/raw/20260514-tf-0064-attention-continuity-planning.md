---
title: TF-0064 Planning Capture — Operational Attention Continuity
date: 2026-05-14
status: raw
type: planning-capture
issue: TF-0064
milestone: M10
---

# TF-0064 Planning Capture

## Issue Goal

Important operational context is not lost during workflow progression.

Scope: operational attention queues, persistent focus state, workflow reminders,
stage-aware operational surfacing.

## Identified Problem

The attention queue (TF-0022/TF-0035) exists only in the Operating Workspace.
The moment a user navigates to Opportunity, Plan Review, or any other workspace,
all visibility into pending items disappears.

If a user is in Plan Review authorizing a plan and there's a critical review
obligation from a previous trade, they have no way to see it without navigating
back to Operating workspace — breaking their cognitive flow.

"Important operational context is not lost during workflow progression" = the
attention state travels with the user, not just lives in one workspace.

## Design

### AttentionSummaryPanel — persistent sidebar widget

A compact panel added to the sidebar (between ActiveDecisionBadge and SessionPanel)
that:
- Fetches `GET /workspaces/operating/attention` independently
- Only renders when a decision is active AND data has loaded
- Shows: item count, urgent item count, top item explanation, "View full queue →" link
- Shows a "Queue clear ✓" state when items.length === 0 (active decision but nothing pending)
- Fails silently on API errors (summary panel is advisory)

### Placement

Sidebar order after TF-0064:
1. WorkspaceNavigation
2. ActiveDecisionBadge
3. AttentionSummaryPanel ← new
4. SessionPanel

### Why sidebar not workspace content

The sidebar is persistent across all workspace navigations (it doesn't re-render on
workspace changes). Placing the attention summary here means it's always visible
regardless of which workspace is active — exactly what "context is not lost" requires.

## Architecture

New file: `frontend/src/workspaces/AttentionSummaryPanel.tsx`
- Props: `params: WorkspaceApiParams`, `onNavigateToOperating: () => void`
- useEffect keyed on `params.decision_id`, `params.persona_id`, `params.workspace_id`
- Clears state when no decision_id (no noise for empty sessions)

Modified: `frontend/src/App.tsx`
- Import AttentionSummaryPanel
- Pass `params` (converted from `context`) and `onNavigateToOperating` callback
- Place in sidebar after ActiveDecisionBadge

Modified: `frontend/src/styles.css`
- `.attention-summary-panel` — compact card style consistent with sidebar
- `.attention-summary-header`, `.attention-summary-icon`, `.attention-summary-count`
- `.attention-summary-urgent-badge` — small amber badge for critical/high items
- `.attention-summary-top` — first item explanation text
- `.attention-summary-link` — "View full queue →" button
- `.attention-summary-clear` — "Queue clear ✓" variant

## Invariants

- AttentionSummaryPanel is advisory/derived — never mutates lifecycle state
- Silent failure: if the attention fetch fails, panel simply doesn't render
- No new API endpoints — reuses existing `GET /workspaces/operating/attention`

## "Queue clear" state

When a decision is active but the attention queue is empty (0 items), the panel
shows "Queue clear ✓" instead of hiding. This is the "workflow reminders" aspect —
the user is explicitly told there's nothing pending, rather than having to wonder
if context was lost.

## Tradeoffs

- Two fetches for the attention queue when on Operating workspace (OperatingWorkspace
  + AttentionSummaryPanel both fetch it). Acceptable: the panel is lightweight and
  the sidebar is persistent. Could be unified in a future refactor.
- No auto-polling — fetches once on context change. The Operating workspace can be
  visited for live updates. Polling would add complexity without clear benefit for MVP.
