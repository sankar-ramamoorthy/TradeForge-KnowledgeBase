---
title: TF-F026 Diagnosis - Compose Master Key Forwarding Gap
type: brainstorm
status: raw
tags: [tradeforge, tf-f026, diagnosis, docker-compose, credentials]
created: 2026-05-17
---

# TF-F026 Diagnosis - Compose Master Key Forwarding Gap

## Observable Gap

The operator can read `TRADEFORGE_MASTER_KEY` in PowerShell, but the Dockerized TradeForge service exits with `MasterKeyNotConfiguredError` during startup.

## Root Cause

`docker-compose.yml` defines `TRADEFORGE_DATABASE_URL` for the `tradeforge` service but does not forward `TRADEFORGE_MASTER_KEY` from the host environment into the container. The container process therefore cannot see the master key even when the invoking shell has it configured.

## Relevant Invariants

- Architectural Simplicity: operator configuration should remain explicit and failure modes should be direct rather than hidden behind restart loops.

## Minimum Correct Scope

Pass `TRADEFORGE_MASTER_KEY` through the Compose boundary, require it at Compose interpolation time, and document the startup expectation for Docker users.

## Out Of Scope

- changing credential encryption or storage
- introducing `.env` support for secrets
- redesigning provider selection or credential lifecycle semantics
