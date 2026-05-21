---
title: TF-F007 Implementation Capture - Credential Setup Documentation
type: raw-implementation-capture
status: raw
created: 2026-05-16
issue: TF-F007
milestone: M10C
---

# TF-F007 Implementation Capture - Credential Setup Documentation

## Runtime Documentation Changes

- Added root `HOW-TO-SETUP-KEYS.md`.
- Added a README reference to the setup guide.
- Added `TRADEFORGE_MASTER_KEY` to `.gitignore` beside `.keys.enc`.
- Updated issue and milestone tracking docs.

## Guide Coverage

- Master-key generation.
- OS-environment setup.
- Registration commands for polygon, alpaca, alpha_vantage, fmp, and finqual.
- Credentialed provider selection through `TRADEFORGE_MARKET_PROVIDER`.
- Provider credential rotation.
- Master-key rotation guidance.
- Revocation handling.
- Explicit non-commit / non-log rules.

## Event / Replay / Lifecycle Notes

- No runtime behavior changed.
- No event model or lifecycle behavior changed.
- The guide explains that replay-oriented credential metadata exists without claiming credential history is already event-sourced.

## Verification

- Manual review against `scripts/manage_credentials.py` command syntax.
- Confirmed `.gitignore` includes `.keys.enc` and `TRADEFORGE_MASTER_KEY`.
- Confirmed `README.md` links to `HOW-TO-SETUP-KEYS.md`.

## Observation

- TF-F007 completes M10C operational readiness: the credential boundary now has a model, storage, composition-root delivery, and an operator setup path.
