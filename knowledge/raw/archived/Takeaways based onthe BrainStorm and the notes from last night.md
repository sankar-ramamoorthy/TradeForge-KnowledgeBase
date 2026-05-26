Takeaways based onthe BrainStorm and the notes from last night (i.e 11:32 pm 2026/05/25)
[[BrainStorm about how to use Litellm and how to have it access Keys]]
[[note-20260526-tf-f075-litellm-injection-blocked-v2]]
Yes — this brainstorm helps a lot.

My read: **you probably do not need Postgres for LiteLLM** if TradeForge is doing provider credential storage, encryption, user authorization, and runtime routing itself. LiteLLM can stay a **stateless internal gateway**. Your note already points to that: LiteLLM should not store user secrets or state; the backend decrypts and injects provider keys per request. 

The key correction for TradeForge:

**Do not use LiteLLM `/config/update` as the main credential-injection mechanism.**

That was the source of the current problem. `/config/update` wants LiteLLM’s DB-backed config machinery. But your better architecture is:

```text
TradeForge backend
  owns encrypted credentials
  owns provider registry
  owns user/model/provider choice
  decrypts per request
  calls LiteLLM /chat/completions with api_key/api_base in payload
```

So the decision is likely:

```text
No LiteLLM DB
No LiteLLM provider secrets
No dynamic config update
No server-level GROQ/OpenAI/NIM keys in LiteLLM env
```

Instead:

```text
LiteLLM = internal stateless protocol adapter
TradeForge = trusted credential + routing boundary
```

This also matches your model-selection brainstorm: provider credentials are stored once per provider, while models are chosen dynamically at runtime. 

The practical implication for TF-F075 is important:

**The feature should pivot from “push decrypted keys into LiteLLM” to “inject decrypted keys into each LiteLLM request.”**

So `injected_at_composition_boundary` may need a rename or semantic change. It should probably mean something like:

```text
resolved_for_runtime_request
```

or:

```text
available_for_runtime_injection
```

not “successfully pushed into LiteLLM gateway.”

Your current TF-F075 finding — that `/config/update` requires LiteLLM’s own database — is real, but it may be telling you that this was the wrong integration path, not that you need to add LiteLLM Postgres.

I would tell Claude/Codex:

```text
Do not add a LiteLLM database yet.

Revise TF-F075 toward stateless LiteLLM operation:
- remove dependency on POST /config/update
- do not store provider keys in LiteLLM
- keep LiteLLM internal-only
- backend decrypts provider key per request
- backend passes api_key/api_base/model dynamically to LiteLLM /chat/completions
- remove server-level provider keys from LiteLLM env unless explicitly configured as operator-owned defaults
- update issue/ADR language so “injection” means request-time composition, not LiteLLM config mutation
```

and from the notes 
[[note-20260526-tf-f075-litellm-injection-blocked-v2]]
This confirms the better decision:

**Back out Option A. Do not give LiteLLM its own Postgres right now.**

Why:

1. Your actual need is not LiteLLM teams, budgets, virtual keys, UI, or usage DB.
2. LiteLLM DB introduces migration/version coupling.
3. `main-latest` made that risk visible immediately.
4. LiteLLM supports passing `api_base` / `api_key` per request, which matches your desired backend-owned credential boundary. ([LiteLLM][1])
5. LiteLLM DB is mainly for things like virtual keys, orgs, teams, budgets, usage tracking, and DB-backed model management. ([LiteLLM][2])

My recommendation:

```text
Decision: Stateless LiteLLM for M13B / TF-F075

Revert:
- scripts/postgres-init/02-litellm-db.sql
- LiteLLM DATABASE_URL
- STORE_MODEL_IN_DB=True
- /config/update admin-client path
- schema/migration docs around LiteLLM DB

Keep:
- LiteLLM as internal Docker service
- LITELLM_MASTER_KEY
- expose: 4000, not ports: 4000:4000
- healthcheck
- backend-owned encrypted provider credentials

Change implementation:
- backend decrypts selected provider key per request
- backend sends model/api_key/api_base dynamically to LiteLLM /chat/completions
- LiteLLM remains stateless
```

Also: **pin the LiteLLM image anyway.** Even stateless services should not use `main-latest`. Use a known version tag and upgrade intentionally.

The issue register should say TF-F075 was blocked because the first design incorrectly coupled TradeForge credential injection to LiteLLM DB-backed dynamic config. The architectural correction is request-time injection, not LiteLLM configuration mutation.

I would tell Codex/Claude:

```text
Abort the LiteLLM DB direction for TF-F075.

Revise TF-F075 to implement stateless request-time LiteLLM credential composition.

TradeForge remains the only credential authority. LiteLLM is an internal stateless gateway. Do not use POST /config/update. Do not require LiteLLM Postgres. Do not store provider credentials or model config in LiteLLM DB.

Also pin the LiteLLM Docker image to a stable explicit version, even though the DB is removed.
```

This is cleaner, safer, and much more aligned with TradeForge doctrine.

[1]: https://docs.litellm.ai/docs/providers/litellm_proxy?utm_source=chatgpt.com "LiteLLM Proxy (LLM Gateway)"
[2]: https://docs.litellm.ai/docs/proxy/db_info?utm_source=chatgpt.com "What is stored in the DB"
