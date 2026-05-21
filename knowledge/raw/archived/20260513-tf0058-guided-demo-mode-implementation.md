---
title: TF-0058 Guided Demo Mode — Implementation
type: raw-implementation
status: raw
issue: TF-0058
milestone: M10
created: 2026-05-13
tags:
  - M10
  - demoability
  - demo
  - seeded-flow
  - implementation
---

# TF-0058: Guided Demo Mode — Implementation

## What Changed

### frontend/src/demo.ts (new)
- DEMO_SEED constant: AAPL, realistic thesis text, realistic plan notes
- runDemoFlow(options): fires 3 API calls in sequence
  1. initNewTradeIdea (Idea stage)
  2. postLifecycleTransition (Thesis)
  3. postLifecycleTransition (Plan)
  Calls setActiveDecision with is_demo: true before returning.
  Returns { decisionId, symbol, record }

### frontend/src/activeDecision.ts
- ActiveDecisionRecord: added is_demo?: boolean

### frontend/src/workspaces/OperatingWorkspace.tsx
- Imports PlayCircle, DEMO_SEED, runDemoFlow
- Added demoLoading + demoError state
- handleStartDemo(): async, calls runDemoFlow, activates decision, navigates to plan-review
- DemoInvitePanel: shown when queue.items is empty AND no active lifecycle stage
  - Shows PlayCircle icon, scenario description (mentions AAPL, seeded to Plan stage)
  - "Start Demo" button with loading state
  - Error display on failure

### frontend/src/operationalLayout.tsx
- ActiveDecisionBadge: shows .demo-badge "Demo" tag when activeDecision.is_demo

### frontend/src/styles.css
- .demo-invite-panel, .demo-invite-header, .demo-invite-description,
  .demo-invite-error, .demo-invite-btn
- .demo-badge (amber/warm coloring to distinguish from lifecycle stage tag)

## Demo Flow

1. Open app — Operating Workspace, no active decision
2. See: "No pending items" + DemoInvitePanel below
3. Read: "See TradeForge in action with a seeded AAPL breakout trade..."
4. Click "Start Demo" → loading state
5. 3 API calls fire: Idea → Thesis → Plan
6. Navigate to Plan Review Workspace
7. Sidebar shows: AAPL / [Plan] [Demo]
8. Progress strip shows ✓Idea ✓Thesis ●Plan
9. WorkflowGuidanceNote: "Review the plan carefully and authorize..."
10. User clicks Authorize Plan → Record Execution → Active Position → ...

## Verification
- 513 tests passed
- tsc, eslint, vite build: clean
- ruff, mypy: clean
