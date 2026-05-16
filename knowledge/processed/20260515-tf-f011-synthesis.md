---
title: TF-F011 Synthesis — Docker Compose Placeholder Command Broke Operational Persistence
type: processed-synthesis
status: processed
tags:
  - TradeForge
  - TF-F011
  - docker
  - persistence
  - operational
  - replayability
created: 2026-05-15
updated: 2026-05-15
source_history:
  - knowledge/raw/20260515-docker-compose-upgrade-notes.md
  - knowledge/raw/20260515-tf-f011-diagnosis.md
  - knowledge/raw/20260515-tf-f011-planning.md
related:
  - "[[Replayability Is Foundational]]"
  - "[[Event Sourcing]]"
  - "[[PostgresEventStore]]"
issues:
  - TF-F011
---

# TF-F011 Synthesis — Docker Compose Placeholder Command Broke Operational Persistence

## What the Feedback Identified

During operational testing on 2026-05-15, a session using SMH decisions produced no entries in the Postgres `event_ledger`. NVDA and NFLX events from prior sessions were present. The current session's events were silently lost.

## Root Cause As Diagnosed

The `command` field in `docker-compose.yml` was a placeholder left from early Docker scaffolding:

```yaml
command: ["uv", "run", "python", "--version"]
```

This exits immediately after printing the Python version. The container stopped. The developer ran the uvicorn server manually outside Docker. The manual invocation environment did not include `TRADEFORGE_DATABASE_URL`. The `create_event_store()` factory resolved to `InMemoryEventStore`. All events from the session were held in process memory and discarded on shutdown.

The `TRADEFORGE_DATABASE_URL` was correctly defined in `docker-compose.yml` — it just never reached the app.

## What Was Changed and Why

Single file changed: `docker-compose.yml`.

1. **Command fixed** — replaced placeholder with uvicorn startup:
   ```yaml
   command: ["uv", "run", "uvicorn", "src.app.api.application:app", "--host", "0.0.0.0", "--port", "8000"]
   ```
   This ensures the app starts inside Docker where the `TRADEFORGE_DATABASE_URL` env var is present.

2. **Port mapping added** — `ports: - "8000:8000"` to the tradeforge service. Without this, the API was only reachable if the server happened to be running manually on the host.

3. **Restart policy added** — `restart: unless-stopped` on both services. Prevents containers from staying down after a host reboot or crash without manual intervention.

## Invariants Involved

**Replayability Is Foundational** — the `event_ledger` is canonical truth. Silent fallback to in-memory state violates this invariant. The fix restores the condition where all operational events reach Postgres regardless of how the server is started.

## Replay and Lifecycle Implications

No event model changes. No lifecycle changes. The fix restores the intended runtime path: `docker compose up` → uvicorn starts with `TRADEFORGE_DATABASE_URL` → `PostgresEventStore` resolves → events written to `event_ledger` → replayable.

## Operational Pattern Note

This gap is a category of operational infrastructure debt: placeholder compose commands left in place after early scaffolding. The `rag-foundry-universal` pattern (documented in the source raw note) delegates server startup to the Dockerfile rather than overriding `command` in compose. Either approach works — what matters is that the compose command is never left as a placeholder in an operational setup.

## Source Traceability

- Source observation: `knowledge/raw/20260515-docker-compose-upgrade-notes.md`
- Diagnosis: `knowledge/raw/20260515-tf-f011-diagnosis.md`
- Planning: `knowledge/raw/20260515-tf-f011-planning.md`
- Runtime change: `docker-compose.yml` — tradeforge command, ports, restart; postgres restart
- Issue: TF-F011 in `DOCS/ISSUE_REGISTER.md`
