---
title: TF-F075 Stateless LiteLLM Request-Time Injection Synthesis
type: processed-synthesis
status: processed
created: 2026-05-26
source:
  - knowledge/raw/note-20260526-tf-f075-litellm-injection-blocked-v2.md
  - knowledge/raw/BrainStorm about how to use Litellm and how to have it access Keys.md
  - knowledge/raw/Takeaways based onthe BrainStorm and the notes from last night.md
archived_source:
  - knowledge/raw/archived/note-20260526-tf-f075-litellm-injection-blocked-v2.md
  - knowledge/raw/archived/BrainStorm about how to use Litellm and how to have it access Keys.md
  - knowledge/raw/archived/Takeaways based onthe BrainStorm and the notes from last night.md
issue: TF-F075
milestone: M13B
tags:
  - tf-f075
  - m13b
  - litellm
  - credential-governance
  - ai-gateway
  - request-time-injection
---

# TF-F075 Stateless LiteLLM Request-Time Injection Synthesis

## Source Context

This synthesis processes three raw notes from the 2026-05-25 evening and
2026-05-26 morning TF-F075 investigation:

```text
knowledge/raw/note-20260526-tf-f075-litellm-injection-blocked-v2.md
knowledge/raw/BrainStorm about how to use Litellm and how to have it access Keys.md
knowledge/raw/Takeaways based onthe BrainStorm and the notes from last night.md
```

The notes were archived after this promotion:

```text
knowledge/raw/archived/note-20260526-tf-f075-litellm-injection-blocked-v2.md
knowledge/raw/archived/BrainStorm about how to use Litellm and how to have it access Keys.md
knowledge/raw/archived/Takeaways based onthe BrainStorm and the notes from last night.md
```

## Synthesis

The TF-F075 investigation exposed a boundary error in the first implementation
direction.

The attempted path treated LiteLLM secret injection as a gateway configuration
mutation problem:

```text
TradeForge CredentialStore
  -> decrypt provider keys
  -> POST /config/update
  -> LiteLLM DB-backed runtime config
  -> provider calls
```

That path forced LiteLLM into DB-backed mode, introduced a separate
`litellm_proxy` database, and coupled TradeForge credential governance to
LiteLLM's internal schema and migration behavior.

The latest notes converge on a cleaner boundary:

```text
TradeForge CredentialStore
  -> backend-owned provider registry
  -> decrypt selected provider key per advisory request
  -> call internal LiteLLM /chat/completions with model/api_key/api_base
  -> provider calls
```

In this model, LiteLLM remains an internal stateless protocol adapter. TradeForge
remains the trusted credential, authorization, provider-selection, and
advisory-governance boundary.

## Root Cause

TF-F075 was blocked because the selected injection mechanism relied on
LiteLLM's DB-backed config machinery rather than TradeForge's existing
composition/request boundary.

The immediate runtime symptom was a LiteLLM schema drift failure:

```text
ERROR: column LiteLLM_MCPServerTable.source_url does not exist
```

The deeper problem was not only an unpinned `main-latest` image. The deeper
problem was that TradeForge had made LiteLLM's mutable configuration database a
required part of provider-secret injection.

That conflicts with the desired managed advisory runtime boundary:

- TradeForge owns encrypted provider credentials.
- TradeForge owns authorization and provider/model selection.
- TradeForge decrypts secrets only inside trusted backend runtime code.
- LiteLLM should not own provider credentials or user-specific state.
- LiteLLM should remain internal-only and stateless for this milestone.

## Corrected Architectural Decision Candidate

For TF-F075, the recommended correction is:

```text
Use stateless LiteLLM request-time credential composition.
```

That means:

- do not use LiteLLM `POST /config/update` as the primary injection mechanism
- do not require a LiteLLM database for M13B
- do not store provider secrets in LiteLLM config or LiteLLM DB
- do not pass server-level Groq/OpenAI/NVIDIA NIM keys into LiteLLM as ordinary
  fallback environment variables unless explicitly modeled as operator-owned
  defaults
- keep LiteLLM internal-only through Docker `expose`, not public `ports`
- keep `LITELLM_MASTER_KEY` as the TradeForge-to-LiteLLM gateway credential
- resolve provider/model/API-base/API-key dynamically in TradeForge
- send the provider key to LiteLLM only in the request that needs it

## Provider And Model Semantics

The stable semantic split is:

```text
provider credential
  -> stored once per provider in TradeForge

model selection
  -> runtime advisory configuration or operator request

request composition
  -> combines provider, model, api_base, and decrypted key
```

Provider credentials are not per-model secrets. Model selection should remain
separate from provider credential storage.

Example provider-registry shape:

```text
provider_id: groq
requires_key: true
model_prefix: groq/
api_base: provider default or configured endpoint

provider_id: ollama
requires_key: false
model_prefix: ollama/
api_base: http://ollama:11434 or host.docker.internal:11434

provider_id: nvidia_nim
requires_key: depends on deployment
model_prefix: openai/ or nvidia_nim/
api_base: configured NIM endpoint
```

The frontend or Provider Governance surface may expose provider and model
selection, but the backend must translate that into the LiteLLM request shape.

## Runtime Implementation Implications

TF-F075 should be revised from "push secrets into LiteLLM" to "compose
credentialed LiteLLM requests."

Likely runtime changes:

- remove `scripts/postgres-init/02-litellm-db.sql`
- remove LiteLLM `DATABASE_URL`
- remove `STORE_MODEL_IN_DB=True`
- remove or de-scope the `/config/update` admin-client path
- remove docs that make LiteLLM DB provisioning part of normal advisory setup
- add a request-time provider credential resolver in the backend advisory path
- extend the OpenAI-compatible provider request builder to include provider
  `api_key` and `api_base` when required
- make governance status distinguish "configured in TradeForge" from
  "available for request-time injection"
- pin the LiteLLM Docker image to an explicit version even when stateless
- preserve the internal-only Docker network boundary
- ensure payloads, headers, and request bodies containing `api_key` are never
  logged

## Terminology Correction

The phrase `injected_at_composition_boundary` became ambiguous during TF-F075.

Under the DB-backed `/config/update` model, it meant:

```text
LiteLLM gateway confirmed receipt of environment variables.
```

Under the corrected stateless model, that should not be the target state.
Better candidate meanings are:

```text
available_for_runtime_injection
resolved_for_runtime_request
configured_for_request_time_composition
```

The important semantic correction is that "injection" should mean request-time
composition by TradeForge, not mutation of LiteLLM's persisted runtime config.

## Security Boundary

Passing a decrypted provider key from the trusted TradeForge backend to an
internal-only LiteLLM container for a single request is acceptable within the
current advisory architecture if these conditions hold:

- LiteLLM is not exposed publicly
- plaintext keys are never stored
- browser clients never receive decrypted keys
- request bodies and headers are sanitized from logs
- LiteLLM debug/request dumping is disabled for normal operation
- `TRADEFORGE_MASTER_KEY` remains backend-only runtime configuration
- provider secrets remain encrypted at rest in `.keys.enc`

This preserves the existing advisory boundary:

```text
Browser
  -> TradeForge backend as trusted credential and governance boundary
  -> internal LiteLLM gateway as stateless adapter
  -> external or local model providers
```

## Operational Synchronization Finding

Runtime documentation currently appears inconsistent with this synthesis.

At processing time, the runtime `DOCS/ISSUE_REGISTER.md` and
`DOCS/adr/0043-managed-ai-gateway-and-governed-llm-provider-secrets.md`
describe TF-F075 as completed through DB-backed LiteLLM `/config/update`
injection. The latest raw notes argue that this should be reversed.

This is a live contradiction, not a resolved doctrine update.

Required follow-up:

- revise TF-F075 in the runtime issue register from completed DB-backed
  injection to a corrective redesign or follow-on corrective issue
- amend ADR-0043 so the accepted runtime injection mechanism is request-time
  composition, not LiteLLM DB-backed config mutation
- update setup docs to remove LiteLLM DB provisioning from the normal path
- update tests to prove request-time credential composition and log-sanitization
- document the rejected DB-backed `/config/update` alternative with the schema
  drift failure and `main-latest` instability as rationale

## Resolution Update 2026-05-26

The runtime contradiction described above has been resolved in the runtime
planning layer.

`DOCS/ISSUE_REGISTER.md` now records TF-F075 as completed with stateless
LiteLLM request-time credential composition. The accepted runtime boundary is:

```text
TradeForge CredentialStore
  -> explicit provider/model advisory selection
  -> request-time credential resolution
  -> internal LiteLLM /chat/completions call
```

The rejected DB-backed `/config/update` path is preserved as rejected history.
TF-F076 subsequently tightened the same managed advisory runtime boundary by
replacing LiteLLM's model-probing `/health` Compose check with
`/health/readiness`.

This synthesis remains processed corrective architecture knowledge; the
runtime issue register and ADR layer are the implementation planning record.

## Invariants Preserved

The corrected direction preserves:

- AI Advisory Boundary: LiteLLM remains advisory infrastructure only.
- Human Decision Sovereignty: no AI output gains lifecycle authority.
- Derived State Must Remain Distinguishable: provider/model status remains
  operational metadata, not canonical decision truth.
- Replayability Is Foundational: replay must not call live LiteLLM or current
  providers to reconstruct historical advisory context.
- Architectural Simplicity: removes unnecessary LiteLLM DB coupling.
- Event Ledger Canonical Truth: no event taxonomy changes are required.

## ADR Implications

ADR-0043 requires amendment if this direction is accepted.

The ADR should preserve the accepted M13B boundary that TradeForge owns
downstream LLM provider secret governance, but it should change the runtime
injection mechanism:

```text
Rejected: LiteLLM DB-backed /config/update as the normal secret-injection path
Accepted: TradeForge request-time composition into internal LiteLLM calls
```

The DB-backed LiteLLM route may remain a documented rejected alternative or a
future option only if TradeForge later needs LiteLLM teams, budgets, virtual
keys, usage tracking, or DB-backed gateway management.

## Canonical Readiness

This synthesis is processed corrective architecture knowledge, not final
canonical doctrine.

It is stable enough to guide the next runtime planning pass, but canonical
promotion depends on resolving the runtime contradiction in ADR-0043 and
TF-F075.
