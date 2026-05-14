---
title: TF-0061 Planning Capture — Operational Onboarding Flow
date: 2026-05-14
status: raw
type: planning-capture
issue: TF-0061
milestone: M10
---

# TF-0061 Planning Capture

## Issue Goal

New users understand the system philosophically before operationally.

Scope: onboarding for lifecycle philosophy, workspace meaning, review-centric workflow,
decision sovereignty, replay concepts.

## Distinction from TF-0060

TF-0060: teaches HOW to use the system — step-by-step through lifecycle actions.
TF-0061: teaches WHY the system works the way it does — philosophical principles first.

The onboarding runs BEFORE any operational interaction. The walkthrough is an operational tool.

## Design

### Trigger

Shown on first visit when `localStorage.getItem("tradeforge.onboarding_complete")` is falsy.
Never shown again after completion or skip.

### Form

Full-screen overlay modal with a centered card. 5 screens, one per principle.
Navigation: Previous / Next (screens 1-4), Get Started (screen 5).
Skip link in top-right corner.

### 5 Philosophical Screens

1. **Decision Sovereignty** — "A decision system, not a trading bot."
   TradeForge does not execute trades, generate signals, or automate decisions.
   Human operators are always in control.

2. **The Decision Lifecycle** — "Every trade follows a structured path."
   Canonical lifecycle: Idea → Thesis → Plan → Approval → Execution → Position → Review.
   Each stage is explicit, auditable, and permanent.

3. **Workspaces Are Cognitive Environments** — "Workspaces are not dashboards."
   Each workspace is focused on a specific decision phase.
   Priority: operational context over market data, workflow continuity over information density.

4. **Review Closes the Learning Loop** — "The review is as important as the trade."
   Review is a first-class workflow. Separates decision process quality from outcome.
   Builds discipline over time regardless of P&L.

5. **Everything Is Replayable** — "Every decision is permanently recorded."
   The event ledger stores every lifecycle transition immutably.
   Any decision can be replayed — exactly what was known, visible, and decided at the time.

## Architecture

### New files
- `frontend/src/onboarding.ts` — `isOnboardingComplete()`, `markOnboardingComplete()`
- `frontend/src/workspaces/OnboardingModal.tsx` — 5-screen modal component with screen data

### Modified files
- `frontend/src/App.tsx` — `onboardingComplete` state, render OnboardingModal before AppShell
- `frontend/src/styles.css` — modal overlay + screen styles

## Storage

`tradeforge.onboarding_complete = "true"` in localStorage.
Set on Get Started or Skip. Never reset automatically.

## Invariants

This is purely UX/informational — no lifecycle events, no API calls, no state mutations.
The onboarding teaches philosophy without gatekeeping any functionality.

## UX Note

The modal should feel like a brief philosophical manifesto, not a tutorial.
Short paragraphs. Bold headlines. Centered. No clutter.
Icons serve as visual anchors, not decorations.
