# LLM Adapter Strategy And Capability Review — 2026-05-22

## Status

Raw note. Authoritative document lives at:

```
TradeForge/DOCS/llm-adapter-strategy.md
```

This note captures the session context and decisions made. Archive after
reading.

---

## Context

M12 was merged. Conducted a full project status review followed by a discussion
about wiring a concrete LLM adapter. The capability gap between M12 advisory
infrastructure and actual advisory intelligence was the central topic.

---

## Key Findings From Capability Review

- M0 through M12 are done and on main.
- The `AIAdvisoryProvider` protocol, service boundaries, and all M12 domain
  models are correct and in place.
- No concrete LLM adapter has been implemented. The advisory layer is an inbox
  with no postman — observations must be manually entered or imported via the
  research artifact API.
- M13 (contextual interpretation) needs a working adapter as upstream input.
  Building M13 before the adapter exists would be premature.

---

## Local Infrastructure (Relevant To Adapter Decision)

- Machine: i7-8565U, 8GB RAM. Runs Ollama with small quantized models (~7B).
  Not suitable for substantive advisory tasks at this quality level.
- LiteLLM: already running locally. Already tested with Groq.
- Groq: free tier, API key available, tested. Provides Llama 3.1 70B — good
  quality for advisory tasks.
- NVIDIA NIM: free tier provisioned, not yet tested. Reportedly better daily
  rate limits than Groq.
- Claude Pro ($20/mo): consumer subscription, NOT Anthropic API access.
- Data sensitivity: currently not sensitive. External API calls acceptable.
  Future sensitivity path documented in the DOCS file.

---

## Decision Made

Implement `OpenAICompatibleAdvisoryProvider` pointed at LiteLLM.

Rationale:
- Single adapter implementation covers all providers (Groq, NVIDIA NIM, Ollama,
  Claude API when added) via LiteLLM config, not code changes.
- LiteLLM already running and tested. Groq free tier is sufficient for on-demand
  single-operator use.
- Credential shape fits existing `CredentialStore` (base_url + api_key + model).
- Model is configurable per task — no hardcoding.

Local Ollama deferred. Could be added later for lightweight structural tasks
(syntax checking, pre-screening) as a LiteLLM routing config change, not a code
change.

---

## Target Advisory Tasks (All Four)

1. Replay summary — `ReplayAdvisoryService` boundary already exists.
2. Thesis review assistant — surfaces blind spots, missing assumptions, regime
   misalignments from structured thesis artifact.
3. Advisory observation generation — given ticker + market context, generates
   typed observations across M12 observation kinds.
4. Candidate screening — ranks/comments on advisory candidate queue contents.

Trigger model: on-demand first. Automatic enrichment on lifecycle events is a
documented future step — hook points to be specified before implementation,
not built now.

---

## Next Issue Candidates

See DOCS file for full ordered list. Top priority:

1. Add `LiteLLMCredential` shape to credential domain.
2. Implement `OpenAICompatibleAdvisoryProvider`.
3. Prompt template for replay summary (lowest risk, validates adapter end-to-end).
4. Prompt template for thesis review (highest operator value).
5. On-demand API endpoints and frontend trigger surfaces.
6. Advisory health check in ProviderConfigurationPanel.
7. Test NVIDIA NIM via LiteLLM and document model string.

---

## What This Does Not Change

- `AIAdvisoryProvider` protocol — unchanged.
- `AIAdvisoryService` boundary enforcement — unchanged.
- ADR-0006 (AI advisory boundary) — still governing.
- Credential architecture (M10C) — adapter credentials go through existing store.
- No new ADR required for the adapter itself.
