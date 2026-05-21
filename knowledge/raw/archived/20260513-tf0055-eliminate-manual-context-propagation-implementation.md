---
title: TF-0055 Eliminate Manual Workspace Context Propagation — Implementation
type: raw-implementation
status: raw
issue: TF-0055
milestone: M10
created: 2026-05-13
tags:
  - M10
  - UX
  - demoability
  - context-propagation
  - implementation
---

# TF-0055: Eliminate Manual Workspace Context Propagation — Implementation

## What Changed

### workspaceRouting.ts
buildWorkspaceHref now only encodes decision_id (when non-empty).
All other params (persona_id, persona_version, workspace_id, selected_workflow_id)
are dropped from URLs. Navigation links are now clean:
- /workspaces/operating
- /workspaces/opportunity?decision_id=<uuid>

readWorkspaceContext still reads decision_id from URL, so replay deep links work.
All workspace API calls still receive full params from App.tsx context object.

### operationalLayout.tsx
Replaced ContextPanel (showing raw IDs) with ActiveDecisionBadge:
- Active: shows symbol prominently + "in workflow" tag + X clear button
- Empty: shows "No active decision — use New Trade Idea to begin" hint
ContextPanelItem kept private (still used by SessionPanel).
WorkspaceBriefing and ContextLink kept as exports but no longer rendered in App.

### App.tsx
- Removed WorkspaceBriefing block (developer meta-commentary)
- Removed ContextLink ("Current routed context" URL artifact)
- ContextPanel → ActiveDecisionBadge in sidebar
- Added handleClearDecision: clearActiveDecision() + setState(null) + navigate to /workspaces/operating
- Removed unused buildWorkspaceHref import and activeHref computed value
- Cleaned up unused lucide-react icon imports (ArrowRight, GitBranch, History, ListChecks)

### styles.css
Added: .active-decision-badge, .active-decision-empty, .active-decision-hint,
.active-decision-header, .active-decision-label, .active-decision-clear,
.active-decision-symbol, .active-decision-meta

## Result

The app now looks like a trading tool:
- Sidebar shows workspace nav + active symbol badge + session panel
- URL bar shows clean paths (no internal routing params leaking to user)
- "Clear" button lets operator close an active decision and return to Operating
- No developer meta-commentary visible in the UI

## Verification
- 513 tests passed (no backend changes)
- ruff, mypy: clean
- tsc, eslint, vite build: clean
