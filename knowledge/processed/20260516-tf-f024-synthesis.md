---
title: TF-F024 Synthesis - Credential Script Direct Invocation Import Failure
type: processed-synthesis
status: processed
tags: [TradeForge, TF-F, feedback, credential-setup, operations]
created: 2026-05-16
updated: 2026-05-16
source_history:
  - knowledge/raw/brainstorm-20260516-credential-setup-import-failure.md
  - knowledge/raw/20260516-tf-f024-diagnosis.md
  - knowledge/raw/20260516-tf-f024-planning.md
related:
  - "[[Architectural Simplicity]]"
issues:
  - TF-F024
---

# TF-F024 Synthesis - Credential Script Direct Invocation Import Failure

## What Feedback Identified

The credential setup guide documented a direct script invocation that failed from the repository root before doing useful work.

## Root Cause

Python direct-file execution placed `scripts/` rather than the repository root on `sys.path`, so `scripts/manage_credentials.py` could not import `src.security` when run exactly as documented.

## What Changed

- Added a small direct-execution bootstrap in `scripts/manage_credentials.py` to insert the repository root into `sys.path` before importing runtime modules.
- Added subprocess regression tests that execute the documented script path for both master-key generation and credential registration.

## Why This Scope Was Correct

The issue was an operational invocation bug, not a credential-boundary design failure. Preserving the documented command and making that path work directly is the minimum correct change.

## Invariants

- Architectural Simplicity: setup commands should remain explicit and reliable for operators.

## Replay / Lifecycle Impact

None.

## Closure

The previously failing documented command now completes successfully from the repository root, and the guide remains accurate without needing wording changes.
