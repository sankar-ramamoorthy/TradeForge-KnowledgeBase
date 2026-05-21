---
title: M10B Planning Capture — Operational Credential Boundary
type: raw-capture
status: raw
tags: [TradeForge, M10B, credential-boundary, security, key-management, planning]
created: 2026-05-15
milestone: M10B
---

# M10B Planning Capture — Operational Credential Boundary

## Context

After completing M10A, we identified a gap in credential management.
External providers currently accept keys as constructor parameters with no
secure storage, rotation, or revocation. The gap was surfaced as a field-observed
architectural concern (TF-F004) and warranted a dedicated milestone.

## Why M10B (Not M12)

M11 introduces AI advisory (LLM provider API calls). You cannot responsibly
start M11 with keys sitting as constructor parameters or raw environment variables.
The credential boundary is a prerequisite for M11, not a cleanup afterwards.

M10B slots cleanly between M10A (cognitive authoring) and M11 (AI advisory).
It's a small, bounded milestone — 4 issues, well-defined scope.

## Provider Landscape

Current active providers:
- yFinance: free, no key
- Polygon.io: api_key
- Alpaca: api_key + secret_key

Planned near-term providers:
- Alpha Vantage: api_key
- FinancialModelingPrep: api_key
- Finqual: api_key

Planned M11+ providers:
- LLM providers (OpenAI, Anthropic, etc.): api_key

## Key Design Decisions

1. TRADEFORGE_MASTER_KEY in OS environment only — not .env, not Git
2. Credential domain model in src/security/ (top-level, not buried in infrastructure)
3. CredentialStore → .keys.enc (Fernet encrypted, gitignored)
4. Composition root (create_app) is sole caller of CredentialStore
5. Provider adapters remain ignorant of encryption — unchanged interface
6. Credential model designed for future replay-safety (status, rotated_at, last_validated_at)

## ADR

ADR-0037 — Operational Credential Boundary and API Key Management
Written and accepted 2026-05-15.

## Issues

TF-F004: Define credential boundary (architectural)
TF-F005: Implement KeyManager + CredentialStore (enhancement)
TF-F006: Wire providers through CredentialStore at composition root (refactor)
TF-F007: Setup guide + documentation (operational)

## Replay Safety Note (Future)

The Credential model's status, rotated_at, last_validated_at fields anticipate
future CredentialValidationEvent artifacts. A replay from 2027 failing because
Polygon expired is operationally meaningful context — the model is designed to
surface that, not just log an error.

Not implemented in M10B. Architecture must not prevent it.
