---
title: TF-F075 Note — LiteLLM Injection Blocked (Second Attempt)
type: note
status: raw
tags: [TradeForge, TF-F075, LiteLLM, credential-governance, blocked]
created: 2026-05-26
issue: TF-F075
milestone: M13B
---

# TF-F075 — Current Blocked State (Second Attempt)

## What Was Done

Option A (Add LiteLLM DB) was implemented:

- `scripts/postgres-init/02-litellm-db.sql` — creates `litellm` user + `litellm_proxy` DB
- `docker-compose.yml` litellm: `DATABASE_URL` + `STORE_MODEL_IN_DB: "True"` + `depends_on: postgres`
- `docker-compose.yml` litellm healthcheck: `start_period` raised from 10s to 60s
- `docker-compose.yml` postgres: mounts `scripts/postgres-init` to `entrypoint-initdb.d`
- ADR-0043 annotated, ISSUE_REGISTER.md updated, HOW-TO-SETUP-KEYS.md updated

## What Was Observed

### First run (fresh volume)

`POST /config/update` returned `400 No DB Connected` → fixed by adding `DATABASE_URL`.

Healthcheck exhausted retries before LiteLLM was ready → fixed by raising `start_period: 60s`.

### Second run (volume preserved)

`POST /config/update` returned `500: "Set STORE_MODEL_IN_DB=True"` → fixed by adding env var.

### Third run (what stopped us)

Postgres error on LiteLLM startup:

```
ERROR: column LiteLLM_MCPServerTable.source_url does not exist
```

LiteLLM's `main-latest` image upgraded between first run (which applied 109 migrations) and
second run. The new image code expects `source_url` on `LiteLLM_MCPServerTable`, but that
column was not in the schema applied during the first run.

This is a **moving-target problem with `main-latest`**. The image version changed between
`docker compose down` and `docker compose up`, pulling a newer image with schema expectations
that don't match the persisted `litellm_proxy` database.

## Root Cause Hypothesis

`main-latest` is unstable as a deployment tag. The older working repo likely used a pinned
LiteLLM version (e.g. `v1.x.x`) that doesn't have this schema drift problem.

## Options for Redesign

1. **Pin LiteLLM to a specific image tag** — e.g. `berriai/litellm:v1.57.0`. Stable schema,
   no surprise upgrades. Check what tag the older working repo used.

2. **Accept the moving-target and always wipe the litellm_proxy DB on upgrade** — document
   that `docker compose down -v` is required when updating the LiteLLM image.

3. **Use `STORE_MODEL_IN_DB=False` and a different injection mechanism** — if model/config
   storage in DB is not needed, check whether `/model/update` (Option B from the original
   handoff) works without DB-backed config storage.

4. **Full redesign** — operator noted older working repo used LiteLLM differently. Understand
   that repo before choosing injection mechanism.

## Files Modified (all uncommitted, on feature/tf-f055-ui-credential-management)

```
docker-compose.yml                           MODIFIED
scripts/postgres-init/02-litellm-db.sql      NEW
DOCS/adr/0043-...md                          MODIFIED
DOCS/ISSUE_REGISTER.md                       MODIFIED
HOW-TO-SETUP-KEYS.md                         MODIFIED
src/infrastructure/advisory/litellm_admin_client.py   NEW (untracked)
src/app/api/application.py                   MODIFIED
src/app/api/routes.py                        MODIFIED
pyproject.toml                               MODIFIED
tests/test_litellm_admin_client.py           NEW (untracked)
tests/test_provider_governance_api.py        MODIFIED
```

## Status

In Progress / Blocked — requires architectural decision on LiteLLM version pinning
and injection mechanism before TF-F075 can be closed.
