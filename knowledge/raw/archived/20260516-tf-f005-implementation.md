---
title: TF-F005 Implementation Capture - Encrypted Local Credential Store
type: raw-implementation-capture
status: raw
created: 2026-05-16
issue: TF-F005
milestone: M10C
---

# TF-F005 Implementation Capture - Encrypted Local Credential Store

## Runtime Changes

- Added `src/security/key_manager.py`.
- Added `src/security/credential_store.py`.
- Extended `src/security/__init__.py` exports.
- Added `scripts/manage_credentials.py` for master-key generation and initial provider registration.
- Added `.keys.enc` to `.gitignore`.
- Added `cryptography` dependency and updated `uv.lock`.
- Added tests for key management and credential-store persistence.
- Updated `DOCS/ISSUE_REGISTER.md` and `DOCS/Milestone_Roadmap_v2.md`.

## Resulting Behavior

- `TRADEFORGE_MASTER_KEY` is loaded from the OS environment only.
- `KeyManager` performs Fernet encryption/decryption over JSON credential payloads.
- Wrong-key decryption raises a bounded error without exposing payload contents.
- `CredentialStore` writes encrypted credential records to `.keys.enc`, reads them back, and filters by `CredentialStatus`.
- Supported initial provider registration targets are polygon, alpaca, alpha_vantage, fmp, and finqual.

## Event / Replay / Lifecycle Notes

- No canonical events added.
- No lifecycle semantics changed.
- Replay-oriented credential metadata remains intact from TF-F004 and is now durably persisted alongside encrypted payloads.

## Verification

- `uv run pytest tests\\test_credential.py tests\\test_key_manager.py tests\\test_credential_store.py` -> 12 passed
- `uv run ruff check src\\security scripts\\manage_credentials.py tests\\test_credential.py tests\\test_key_manager.py tests\\test_credential_store.py` -> clean
- `uv run mypy src\\security scripts\\manage_credentials.py tests\\test_credential.py tests\\test_key_manager.py tests\\test_credential_store.py` -> clean

## Deferred Work

- TF-F006: composition-root wiring through `CredentialStore`
- TF-F007: setup guide, rotation workflow, and stronger operational documentation

## Observations

- The minimal `argparse` script is enough for the current milestone and avoids introducing a broader CLI framework before there is demonstrated need.
- The implementation preserves the intended boundary: provider adapters still do not import from `src/security/`.
