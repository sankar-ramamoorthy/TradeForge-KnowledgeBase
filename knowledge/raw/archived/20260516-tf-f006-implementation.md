---
title: TF-F006 Implementation Capture - Provider Credential Wiring
type: raw-implementation-capture
status: raw
created: 2026-05-16
issue: TF-F006
milestone: M10C
---

# TF-F006 Implementation Capture - Provider Credential Wiring

## Runtime Changes

- Added optional `credential_store` injection to `create_app()`.
- Added local composition helpers for default credential-store discovery and market-provider construction.
- Added `TRADEFORGE_MARKET_PROVIDER` provider selection with `yfinance` as the default keyless provider.
- Routed `polygon` and `alpaca` provider construction through decrypted credential payloads at the composition root only.
- Added `tests/test_provider_credential_wiring.py` integration coverage through `/workspaces/market-context`.
- Updated issue and milestone tracking docs.

## Resulting Behavior

- `yfinance` continues to work without credentials.
- `polygon` and `alpaca` require credential records and receive only decrypted constructor values inside the composition root.
- Provider adapters remain unaware of `src/security/`.
- Future credentialed provider adapters have a single composition-root registry to join rather than inventing raw environment-variable wiring.

## Event / Replay / Lifecycle Notes

- No canonical events added.
- No lifecycle behavior changed.
- Provider capability delivery remains operational infrastructure, not event-ledger truth.

## Verification

- `uv run pytest tests\\test_provider_credential_wiring.py tests\\test_market_context_overlay.py` -> 20 passed
- `uv run ruff check src\\app\\api\\application.py tests\\test_provider_credential_wiring.py` -> clean
- `uv run mypy --follow-imports=skip src\\app\\api\\application.py tests\\test_provider_credential_wiring.py` -> clean
- Provider adapter import scan confirmed zero `src.security` imports.

## Deferred Work

- TF-F007: operator-facing setup and rotation documentation.
- Future provider adapter implementations for Alpha Vantage, FMP, and Finqual.

## Observation

- Full unscoped mypy traversal still reports an unrelated pre-existing missing-return issue in `src/services/workspace_engine/attention.py`; TF-F006 files are clean under focused checking.
