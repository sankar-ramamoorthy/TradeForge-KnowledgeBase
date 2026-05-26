---
title: TF-F075 Handoff — LiteLLM Secret Injection Blocked
type: handoff
status: raw
tags: [TradeForge, TF-F075, LiteLLM, credential-governance, handoff, blocked]
created: 2026-05-26
issue: TF-F075
milestone: M13B
branch: feature/tf-f075-litellm-secret-injection
---

# TF-F075 Handoff — LiteLLM Secret Injection Blocked

## Purpose

This document captures the full state of TF-F075 as of 2026-05-26 for a fresh
context. The implementation hit a hard architectural constraint during end-to-end
validation. A decision on the injection mechanism is required before work can
continue. The operator's choice is recorded at the bottom of this document.

---

## Background

TF-F074 (Done, M13B) implemented encrypted credential storage for LLM provider
keys (`llm_groq`, `llm_nvidia_nim`, `llm_openai`, `llm_anthropic`, `llm_google`).
The keys are stored in `.keys.enc` and decrypted into
`app.state.litellm_provider_environment` at composition time. However, that dict
was only used for governance display. The `injected_at_composition_boundary` badge
said "injected" whenever TradeForge decrypted the key internally — it did NOT
mean LiteLLM was actually using the key. This was the gap.

TF-F075 was created to close that gap.

---

## What TF-F075 Implemented (all in place on branch)

### 1. LiteLLMGatewayAdminClient
**File:** `src/infrastructure/advisory/litellm_admin_client.py`

New class. Calls `POST {litellm_base_url}/config/update` with
`{"environment_variables": {...}}` authenticated via the `litellm` credential's
`api_key` (which is the LiteLLM master key). Returns bool. Never raises. Never
logs secrets. 5-second timeout.

### 2. _push_to_litellm_gateway() helper
**File:** `src/app/api/application.py`

New module-level function. Gets `litellm` credential from store, builds
`LiteLLMGatewayAdminClient`, calls `push_environment()`. Returns
`frozenset[str]` of provider_ids confirmed pushed, or `frozenset()` on failure.
Stored in `app.state.litellm_gateway_active_providers`.

Called from two places:
- `ProviderBootstrapService.reload()` — after every credential save
- `create_app()` startup — to push existing `.keys.enc` credentials on boot

### 3. Display semantics fix
**File:** `src/app/api/routes.py`, function `_build_llm_provider_secret_injection_response()`

Changed `injected_at_composition_boundary` to read from
`app.state.litellm_gateway_active_providers` instead of
`app.state.litellm_provider_environment`. The badge now means "LiteLLM runtime
confirmed receipt" not "TradeForge decrypted internally."

### 4. docker-compose ordering
**File:** `docker-compose.yml`

Added LiteLLM healthcheck (probes `GET /health` with `Authorization: Bearer
$LITELLM_MASTER_KEY`). Added `depends_on: litellm: condition: service_healthy,
required: false` to tradeforge service. TradeForge now waits for LiteLLM to be
healthy before starting. `required: false` means non-advisory deployments are
unaffected.

### 5. pyproject.toml
Added `httpx>=0.28` as explicit direct dependency.

### 6. Tests
- `tests/test_litellm_admin_client.py` — 6 new unit tests (success, connection
  error, HTTP status error, empty dict early-return, URL normalization, unexpected
  exception)
- `tests/test_provider_governance_api.py` — existing test updated; new test added
  for gateway-active path

### 7. Documentation
- ADR-0043 annotated with TF-F075 implementation note and TF-F075 added to
  `related_issues`
- ISSUE_REGISTER.md: TF-F075 registered (table row + full entry, status: In
  Progress / Blocked)

---

## The Blocking Constraint

**What was observed during end-to-end validation:**

```
litellm-1: Exception occured - No DB Connected
litellm-1: POST /config/update HTTP/1.1" 400 Bad Request
tradeforge-1: LiteLLM gateway returned 400; provider secrets not pushed.
```

**Root cause:** `POST /config/update` in LiteLLM proxy requires LiteLLM's own
database (`DATABASE_URL` configured) to persist config changes. In config-file-only
mode (no DB), the endpoint returns 400. There is no way to inject environment
variables into the running LiteLLM process via Admin API without its DB.

**What this means:** `LiteLLMGatewayAdminClient.push_environment()` always
returns False. `injected_at_composition_boundary` will never be True.
TF-F075's core acceptance criterion is not met.

**What IS working:**
- docker-compose ordering (litellm healthy before tradeforge starts) ✓
- LiteLLM healthcheck authentication (200 OK on probes) ✓
- Display semantics corrected (badge no longer falsely claims injection) ✓
- 46/46 tests pass ✓

---

## ADR-0043 Constraint

ADR-0043 §Downstream LLM Provider Secrets states:
> "plaintext secrets must not be logged, returned, committed, or embedded in
> static LiteLLM config"

This constrains the solution space. Any approach that writes plaintext API keys
to a persistent file violates this. Transient/in-memory materialization may be
acceptable if it does not persist.

ADR-0043 §Runtime Injection states:
> "TradeForge may inject decrypted secrets into managed LiteLLM during startup
> or reload."

This explicitly authorizes injection — the mechanism is what needs to be chosen.

---

## Four Options (decision required)

### Option A — Add LiteLLM DB
Connect LiteLLM to a postgres database so `/config/update` works.

**What it requires:**
- Add `DATABASE_URL: postgresql://litellm:litellm@postgres:5432/litellm_proxy` to
  litellm service in docker-compose.yml
- Create the `litellm_proxy` database in the existing postgres container (or
  separate service)
- LiteLLM runs DB migrations automatically on first start
- `POST /config/update` then works; `push_environment()` succeeds at runtime

**Trade-offs:**
- Clean dynamic injection, no restart needed
- Adds DB dependency to litellm (more infra to manage)
- LiteLLM's own DB data persists separately from TradeForge's postgres data
- Can share the existing postgres instance (different DB name)

**Key implementation:** Add `LITELLM_DATABASE_URL` env var to litellm service.
LiteLLM uses this for its own persistence (spend logs, config, etc.).

---

### Option B — Per-model injection via POST /model/update
LiteLLM has a model-level update endpoint that MAY work without DB (unverified).

**What it requires:**
- Replace `POST /config/update` call with `POST /model/update` calls, one per
  provider model (e.g. `tradeforge-nim-70b` → set `litellm_params.api_key`)
- Verify whether this endpoint requires DB in LiteLLM no-DB mode
- Map provider_id → model alias(es) (e.g. `llm_nvidia_nim` → `tradeforge-nim-70b`)
  using `litellm_config.yaml` model names

**Trade-offs:**
- May work without DB (needs verification against running container)
- More complex mapping: one provider may have multiple model aliases
- If it also requires DB, same constraint as Option A

**Verification command against running container:**
```
curl -X POST http://localhost:4000/model/update \
  -H "Authorization: Bearer sk-tradeforge-local-dev" \
  -H "Content-Type: application/json" \
  -d '{"model_id":"tradeforge-nim-70b","litellm_params":{"api_key":"test"}}'
```

---

### Option C — Accept host env var model (no dynamic injection)
Accept that LiteLLM reads provider keys from host environment at startup only.
TradeForge's credential store governs its own masking, rotation, provenance, and
display. The "injection" into LiteLLM is the operator's responsibility via host
env vars before `docker compose up`.

**What it requires:**
- Revert `LiteLLMGatewayAdminClient` push logic (or leave it as a no-op)
- Correct the display: `injected_at_composition_boundary` permanently False
  (or remove the concept and replace with `configured_in_tradeforge`)
- Update UI labels to be honest: "stored" not "injected"
- Document in HOW-TO-SETUP-KEYS.md that provider keys for LiteLLM must be set
  as host env vars: `export NVIDIA_NIM_API_KEY=...` before `docker compose up`
- The existing `${NVIDIA_NIM_API_KEY:-}` in docker-compose.yml already handles this

**Trade-offs:**
- Simplest infrastructure (no new DB, no admin API calls)
- Honest about what TradeForge actually does
- Does NOT satisfy ADR-0043 "secrets injected into LiteLLM only through managed
  runtime" — host env vars bypass TradeForge governance
- ADR-0043 may need to be revised to reflect this boundary

---

### Option D — Materialize litellm_config.yaml at composition time
TradeForge generates a resolved `litellm_config.yaml` with actual API key values
(not `os.environ/` references) written to a shared volume. LiteLLM reads it.

**What it requires:**
- Change litellm volume mount from read-only to read-write (or shared named volume)
- TradeForge writes `/app/litellm_config.yaml` at startup/credential-save with
  actual keys embedded in `litellm_params.api_key` fields
- LiteLLM must be restarted (or a hot-reload triggered) to pick up the new file
- File must be ephemeral (not committed, not persistent across rebuilds)

**Trade-offs:**
- Conceptually simple
- Requires litellm restart on credential change (no hot-reload without DB)
- Plaintext keys in a file on shared volume — ADR-0043 caution applies
  (acceptable only if file is truly ephemeral and not committed)
- Docker SDK or docker-compose restart command needed for litellm restart

---

## Operator's Decision

**Chosen option:** A — Add LiteLLM DB (existing postgres, separate database/schema)

**Rationale:**
LiteLLM's docs treat `DATABASE_URL` as the normal proxy deployment path and
dynamic model/config management is DB-backed by design. Option C ("host env
vars") would weaken M13A's core value: UI credential setup that actually affects
the running gateway. `/config/update` is the correct mechanism; it just needs
LiteLLM's own DB to be present.

**Implementation decision:**
Use the existing postgres service (`tradeforge-postgres-1`) but isolate LiteLLM
storage from TradeForge domain/event data. Create a separate database (e.g.
`litellm_proxy`) and a separate postgres user (`litellm`) with restricted
access. LiteLLM runs its own DB migrations automatically on first start.

**ADR impact:**
ADR-0043 §Runtime Injection needs a brief annotation clarifying that dynamic
injection via `/config/update` requires LiteLLM to be running in DB-backed mode
(`DATABASE_URL` configured). No semantic change — the injection mechanism is
confirmed, the DB dependency is the new constraint to document.

**Option B (per-model update) note:**
Only pursue `/model/update` if a quick one-shot spike against the running
container confirms it works without DB. If it requires DB too (likely), skip
directly to Option A. Do not invest implementation time speculatively.

---

## Files Modified in TF-F075 (for fresh context)

```
src/infrastructure/advisory/litellm_admin_client.py   NEW
src/app/api/application.py                            MODIFIED
src/app/api/routes.py                                 MODIFIED
pyproject.toml                                        MODIFIED
docker-compose.yml                                    MODIFIED
DOCS/adr/0043-managed-ai-gateway-and-governed-llm-provider-secrets.md  MODIFIED
DOCS/ISSUE_REGISTER.md                                MODIFIED
tests/test_litellm_admin_client.py                    NEW
tests/test_provider_governance_api.py                 MODIFIED
```

## Key State in application.py

```python
# _push_to_litellm_gateway() — module-level helper after _default_litellm_provider_environment()
# Called from:
#   ProviderBootstrapService.reload()  (line ~340)
#   create_app()  (line ~540)
# Returns frozenset[str] of provider_ids, stored as app.state.litellm_gateway_active_providers
```

## Key State in routes.py

```python
# _build_llm_provider_secret_injection_response() reads:
#   gateway_active = getattr(request.app.state, "litellm_gateway_active_providers", frozenset())
#   injected = schema.provider_id in gateway_active
# (not litellm_provider_environment)
```

## Current Branch

`feature/tf-f075-litellm-secret-injection` — all changes committed except the
decision on injection mechanism. The branch diverges from `main` at commit
`61adfae` (M13A/M13B milestone commit).

## Test Command

```
uv run pytest tests/test_litellm_admin_client.py tests/test_provider_governance_api.py tests/test_admin_credentials.py tests/test_credential_store.py tests/test_llm_provider_secrets.py -v
```

All 46 pass against current branch state.
