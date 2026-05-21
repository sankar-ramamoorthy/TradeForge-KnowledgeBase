---
title: TF-F011 Diagnosis — Docker Compose Placeholder Command Breaks Operational Persistence
type: raw-diagnosis
status: raw
tags:
  - TradeForge
  - TF-F011
  - docker
  - persistence
  - operational
  - diagnosis
created: 2026-05-15
updated: 2026-05-15
source: knowledge/raw/20260515-docker-compose-upgrade-notes.md
---

# TF-F011 Diagnosis — Docker Compose Placeholder Command Breaks Operational Persistence

## Observable Gap

During operational testing on 2026-05-15, the TradeForge runtime container (`tradeforge-tradeforge-1`) was found stopped. The API server was running manually outside Docker without `TRADEFORGE_DATABASE_URL` set in the environment. Events created in the session (SMH decisions) were not present in the Postgres `event_ledger` table. Events from prior sessions (NVDA, NFLX) were present — confirming in-memory fallback was active for the current session only.

## Root Cause

The `command` field in `docker-compose.yml` is a placeholder:

```yaml
command: ["uv", "run", "python", "--version"]
```

This starts a Python process, prints the Python version string, and immediately exits. The Docker container exits cleanly with code 0 after printing the version. The uvicorn server never starts. No HTTP API is served.

Because the container exited, the developer ran the server manually outside Docker. The manual invocation did not have `TRADEFORGE_DATABASE_URL` in its environment, so `create_event_store()` resolved to the `InMemoryEventStore` fallback. All events written during the session were held in process memory and discarded on shutdown.

## Why This Is an Invariant Violation

**Replayability Is Foundational** — the event ledger is canonical truth. Silent fallback to in-memory state violates replayability and breaks operational continuity. Events written during this session cannot be replayed because they were never persisted.

**This is not a race condition or intermittent bug.** The container exits every time it starts, because the placeholder command is the actual configured command. The developer workaround masked this gap.

## What Is Not the Root Cause

- `TRADEFORGE_DATABASE_URL` is correctly defined in `docker-compose.yml` under the `tradeforge` service environment block.
- The Postgres service and healthcheck are correctly configured and functioning.
- The `InMemoryEventStore` fallback behavior is architecturally correct — it is the behavior when no DB URL is present in the process environment.
- The in-memory fallback itself is not the bug. The bug is that the server never ran inside Docker where the URL was available.

## Minimum Correct Scope of Change

Three changes to `docker-compose.yml`, no code changes required:

1. **Fix `command`** — replace the Python version placeholder with the uvicorn startup command.
2. **Add `ports: - "8000:8000"`** — the tradeforge service had no port mapping, making the manually-started external server the only accessible endpoint.
3. **Add `restart: unless-stopped`** — prevents the container from staying stopped after a host reboot or crash.

Optional (deferred per raw note):
- `.env` file support — not urgent; `TRADEFORGE_DATABASE_URL` is low-sensitivity local dev credentials already in compose.
- Named bridge network — cosmetic improvement, not affecting persistence.

## Scope Boundary

Out of scope for TF-F011:
- `.env` file support
- Named bridge network
- Application-level startup logging to confirm active event store backend (separate TF-F issue if needed)
- Any changes to application code
