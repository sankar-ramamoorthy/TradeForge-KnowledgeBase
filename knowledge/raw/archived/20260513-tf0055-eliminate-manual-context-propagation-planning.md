---
title: TF-0055 Eliminate Manual Workspace Context Propagation — Planning Capture
type: raw-planning
status: raw
issue: TF-0055
milestone: M10
branch: feature/tf-0055-eliminate-manual-context-propagation
created: 2026-05-13
tags:
  - M10
  - UX
  - demoability
  - context-propagation
  - planning
---

# TF-0055: Eliminate Manual Workspace Context Propagation — Planning Capture

## What Remains After TF-0053 + TF-0054

TF-0054 solved: active decision_id flows automatically into all workspace API calls.
TF-0055 problem: the UI still looks like an engineering system in three ways:

1. Navigation URLs expose all internal routing params:
   /workspaces/opportunity?persona_id=persona.swing&persona_version=2026-05-11
   &workspace_id=workspace.operating&decision_id=<uuid>
   Traders never need to see or manage these.

2. ContextPanel in sidebar shows raw internal IDs:
   Persona: persona.swing / Persona Version: 2026-05-11 / Decision: decision.focus
   This is developer debugging info, not operational context.

3. WorkspaceBriefing at top of app is developer meta-commentary:
   "Operational session context" / "Six MVP routes" / "Context preserved"
   ContextLink "Current routed context" is a developer URL artifact.

## Implementation Plan

### workspaceRouting.ts
`buildWorkspaceHref` — only include decision_id when non-empty.
All other params (persona_id, persona_version, workspace_id, selected_workflow_id)
come from session + localStorage now — no need to encode in URL.

Clean URL examples:
- /workspaces/operating  (no active decision)
- /workspaces/opportunity?decision_id=<uuid>  (with active decision)

### operationalLayout.tsx
Replace ContextPanel with ActiveDecisionBadge:
- Props: activeDecision: ActiveDecisionRecord | null, onClear: () => void
- Shows: symbol + "Active" tag when decision set, prompt otherwise
- Clear button resets active decision context

Remove WorkspaceBriefing from exports (or leave it but stop using it in App).
Remove ContextLink (or leave export, remove from App.tsx usage).

### App.tsx
- Remove WorkspaceBriefing block + unused icon imports
- Remove ContextLink
- Replace ContextPanel with ActiveDecisionBadge
- Pass activeDecision prop to sidebar
- Add handleClearDecision: clearActiveDecision() + setActiveDecisionState(null) + navigate to /workspaces/operating

### CSS
- .active-decision-badge styles
- Remove/adjust .workspace-briefing if no longer rendered

## ADR Checkpoint

No new ADR. Navigation URL structure is an implementation detail within existing
ADR-0021 (React workspace runtime boundary).

## Context resolution remains correct

When navigating to /workspaces/opportunity (clean URL):
- readWorkspaceContext("") → {}
- mergeWorkspaceContext({}, session + activeDecision) → full context with decision_id
- API calls still send persona_id etc. (from context, not URL)
- Backend still gets all required params

URL params serve only as overrides (replay deep links, shared links).

## Tests

Backend: no new tests needed (API contract unchanged).
Verify clean URL navigation doesn't break existing 513 tests.
