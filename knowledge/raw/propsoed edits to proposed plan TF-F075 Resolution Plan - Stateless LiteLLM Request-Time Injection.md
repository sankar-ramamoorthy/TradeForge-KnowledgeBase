[[TF-F075 Resolution Plan - Stateless LiteLLM Request-Time Injection.md]]
Yes — I would approve this plan with **three edits**.

First, replace this phrase:

> infer required provider secret from model/provider prefix

with:

> resolve provider from explicit provider field where available; fall back to model prefix only for legacy/internal calls.

Do not make model strings your only authority. `openai/...`, `nvidia_nim/...`, `ollama/...` are routing syntax, not governance identity.

Second, split the runtime work into two pieces:

```text
LLMProviderCredentialResolver
  -> provider identity
  -> requires_key
  -> encrypted credential lookup
  -> decrypted secret or unavailable

LiteLLMRequestComposer
  -> model
  -> api_base
  -> api_key if required
  -> messages/params
```

That gives you better tests and keeps secret handling isolated.

Third, change “Replace OpenAI SDK calls” to:

> Replace advisory OpenAI-compatible calls currently bypassing LiteLLM with an internal LiteLLM gateway adapter.

That avoids overcommitting if some test/fake/provider code still uses OpenAI-shaped abstractions.

The LiteLLM docs support your new direction: per-request `api_key` / `api_base` is documented, while DB-backed features are for virtual keys, organizations, teams, budgets, and usage tracking. ([LiteLLM][1]) Also, pinning is right; LiteLLM’s own release guidance distinguishes rolling tags from reproducible pinned versions. ([LiteLLM][2])

My final instruction to Codex would be:

```text
Proceed with TF-F075 stateless request-time LiteLLM injection.

Important corrections:
1. Prefer explicit provider identity over inferring provider only from model prefix.
2. Introduce separate credential-resolution and request-composition seams.
3. Remove LiteLLM DB/config-update work completely.
4. Pin LiteLLM to an explicit reproducible version.
5. Keep LiteLLM internal-only and secret-free.
```

This is the right M13B correction.

[1]: https://docs.litellm.ai/docs/providers/litellm_proxy?utm_source=chatgpt.com "LiteLLM Proxy (LLM Gateway)"
[2]: https://docs.litellm.ai/blog/cleaner-release-versions?utm_source=chatgpt.com "standard names, MINOR for weekly, PATCH for hotfixes"
