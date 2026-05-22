# Docker Compose Upgrade Notes — 2026-05-15

## Context

During operational testing on 2026-05-15, the TradeForge runtime container (`tradeforge-tradeforge-1`) was found to be stopped. The current `docker-compose.yml` has a placeholder `command` that just prints the Python version and exits — it does not start the uvicorn server. As a result, the server was being run manually outside Docker, without `TRADEFORGE_DATABASE_URL` in the environment, causing the in-memory event store to be used instead of Postgres. Events were not persisted across sessions.

This was discovered by querying the event ledger directly:

```sql
SELECT ledger_sequence, event_type, payload->>'symbol' AS symbol, entity_references
FROM event_ledger
ORDER BY occurred_at DESC;
```

The ledger showed NVDA and NFLX events from earlier sessions but no SMH events from the current session — confirming the in-memory fallback was active.

---

## Current docker-compose.yml gaps

Compared to the rag-foundry-universal pattern, the TradeForge compose is missing:

1. **Server start command** — `command` is `["uv", "run", "python", "--version"]` (placeholder). Should be the uvicorn startup command.
2. **`restart: unless-stopped`** — container does not recover after crash or host reboot.
3. **`env_file`** — no `.env` file support. Secrets are either hardcoded in compose or missing entirely. rag-foundry used `env_file: - ./.env` to keep secrets out of the compose file.
4. **No explicit network** — rag-foundry defined a named bridge network (`ragnet`) for clean inter-service isolation. TradeForge uses Docker's implicit default network.

---

## Reference: rag-foundry-universal pattern

File: `C:\Users\bosto\dockerstuff\rag-foundry-universal\docker-compose.yml`

Key patterns to adopt:

```yaml
restart: unless-stopped

env_file:
  - ./.env

environment:
  DATABASE_URL: postgresql://user:pass@postgres:5432/db

networks:
  - ragnet

networks:
  ragnet:
    driver: bridge
```

Each service in rag-foundry has its own Dockerfile that defines the actual server start — compose does not override `command`. TradeForge should do the same or set `command` explicitly in compose.

---

## Proposed TradeForge compose changes (deferred)

When ready to implement:

- Fix `command` to start the uvicorn server: `["uv", "run", "uvicorn", "src.app.api.application:app", "--host", "0.0.0.0", "--port", "8000"]`
- Add `restart: unless-stopped` to the tradeforge service
- Add `env_file: - ./.env` and move `TRADEFORGE_DATABASE_URL` there (or keep in compose — low sensitivity since it's local dev credentials)
- Optionally add a named network for explicitness

The `TRADEFORGE_DATABASE_URL` is already correctly defined in compose — it just never reaches the server because the server is not started by compose.

---

## Postgres connection details (local dev)

Container name: `tradeforge-postgres-1`
User: `tradeforge`
Database: `tradeforge`

Connect:
```
docker exec tradeforge-postgres-1 psql -U tradeforge -d tradeforge
```

Tables:
- `event_ledger` — canonical append-only event store
- `market_advisory_snapshots` — market context snapshots
- `alembic_version` — migration tracking

---

## Status

Deferred. No code changes made. This note captures the gap for future implementation.


Captured. This is a good **raw note** and should become an issue cluster, not just a TODO.

Suggested GitHub issues:

1. **Fix Docker Compose server startup**

   * Replace placeholder command with uvicorn startup.
   * Verify API starts inside Docker.

2. **Ensure runtime uses Postgres event store**

   * Confirm `TRADEFORGE_DATABASE_URL` reaches app.
   * Add startup/logging check showing active event store backend.

3. **Add `.env` support for local Docker config**

   * Add `env_file: ./.env`.
   * Document required variables.

4. **Harden local compose runtime**

   * Add `restart: unless-stopped`.
   * Optionally add named bridge network.

5. **Add persistence regression test / operational check**

   * Create event via API.
   * Restart container.
   * Verify event remains in `event_ledger`.

Canonical framing:

```text
This is not just Docker cleanup.
This is an operational persistence integrity issue.

If the server can silently fall back to in-memory state because Docker is misconfigured,
then replayability and event-sourced truth are compromised.
```

Maybe file this under something like:

 a dedicated technical issue cluster:

```text
TF-Docker Runtime Reliability
```

btw.. here is another review.

your env var injection is already correct here:

environment:
  TRADEFORGE_DATABASE_URL: postgresql://tradeforge:tradeforge@postgres:5432/tradeforge

The real problem is this line:

command: ["uv", "run", "python", "--version"]

That starts Python, prints the version, then exits. So the TradeForge app never runs inside Docker.

Change it to:

command:
  [
    "uv",
    "run",
    "uvicorn",
    "src.app.api.application:app",
    "--host",
    "0.0.0.0",
    "--port",
    "8000"
  ]

Also add the API port:

ports:
  - "8000:8000"

A corrected minimal version:

services:
  tradeforge:
    image: tradeforge-runtime:local
    build:
      context: .
      dockerfile: Dockerfile
    working_dir: /app
    environment:
      TRADEFORGE_DATABASE_URL: postgresql://tradeforge:tradeforge@postgres:5432/tradeforge
    ports:
      - "8000:8000"
    volumes:
      - .:/app
      - tradeforge-venv:/app/.venv
    depends_on:
      postgres:
        condition: service_healthy
    restart: unless-stopped
    command:
      [
        "uv",
        "run",
        "uvicorn",
        "src.app.api.application:app",
        "--host",
        "0.0.0.0",
        "--port",
        "8000"
      ]

  postgres:
    image: postgres:16-bookworm
    environment:
      POSTGRES_DB: tradeforge
      POSTGRES_USER: tradeforge
      POSTGRES_PASSWORD: tradeforge
    ports:
      - "5432:5432"
    volumes:
      - tradeforge-postgres-data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U tradeforge -d tradeforge"]
      interval: 5s
      timeout: 5s
      retries: 10
    restart: unless-stopped

volumes:
  tradeforge-venv:
  tradeforge-postgres-data:

You do not need .env yet. That can come later. Right now the urgent fix is: start uvicorn inside Docker so the existing TRADEFORGE_DATABASE_URL is actually used.