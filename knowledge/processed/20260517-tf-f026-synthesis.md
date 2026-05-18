---
title: TF-F026 Synthesis - Compose Master Key Forwarding Gap
type: processed-synthesis
status: processed
tags: [TradeForge, TF-F, feedback, docker-compose, credentials]
created: 2026-05-17
updated: 2026-05-17
source_history:
  - knowledge/raw/brainstorm-20260517-compose-master-key-forwarding.md
  - knowledge/raw/20260517-tf-f026-diagnosis.md
  - knowledge/raw/20260517-tf-f026-planning.md
related:
  - "[[Architectural Simplicity]]"
issues:
  - TF-F026
---

# TF-F026 Synthesis - Compose Master Key Forwarding Gap

## What Feedback Identified

The operator had configured `TRADEFORGE_MASTER_KEY` in PowerShell, but the Dockerized runtime still failed as if the key were absent.

## Root Cause

The host shell contained the key, but `docker-compose.yml` did not pass it into the `tradeforge` service environment.

## What Changed

- Forwarded `TRADEFORGE_MASTER_KEY` into the runtime container through Compose interpolation.
- Added a required-variable guard so missing host configuration fails at Compose evaluation time.
- Updated the credential setup guide with the Compose-specific startup requirement.

## Why This Scope Was Correct

The credential system itself was working as designed. The issue was a deployment-boundary omission, so changing the Compose contract and docs resolves the observed failure without widening the credential architecture.

## Invariants

- Architectural Simplicity: configuration should be explicit and failures should surface at the boundary where they originate.

## Replay / Lifecycle Impact

None.

## Closure

The original observation is resolved when Compose is launched from a shell that contains `TRADEFORGE_MASTER_KEY`: the rendered service environment now includes the key instead of omitting it.
