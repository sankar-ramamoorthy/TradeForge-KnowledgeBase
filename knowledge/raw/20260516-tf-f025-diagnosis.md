---
title: TF-F025 Diagnosis - Runtime API Unavailable Startup Fan-Out
type: raw-diagnosis
status: raw
tags: [TradeForge, TF-F, feedback, diagnosis, frontend]
created: 2026-05-16
updated: 2026-05-16
source_history:
  - knowledge/raw/20260516 feed back Bug.md
issues:
  - TF-F025
---

# Observable Gap

Starting the frontend while the runtime API is unavailable produces repeated Vite proxy failures for `/health`, `/session`, `/workspaces/*`, and `/lifecycle/decisions`.

# Root Cause

The React application performs a health check, but health is informational only. The rest of the application still mounts immediately and each workspace or sidebar component begins its own API calls even when the runtime boundary has not been proven available.

# Relevant Invariants

- UX Is Architectural
- Architectural Simplicity

# Minimum Correct Scope

Add a single application-level runtime-availability gate in the frontend so operational workspace components do not mount until the API boundary is confirmed available.

# Explicitly Out Of Scope

- Backend startup changes
- Automatic backend orchestration from the frontend
- Redesigning the local two-terminal development workflow
- Broader request retry infrastructure
