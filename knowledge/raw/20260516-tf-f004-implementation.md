---
title: TF-F004 Implementation Capture - Credential Domain Model
type: raw-implementation-capture
status: raw
created: 2026-05-16
issue: TF-F004
milestone: M10C
---

# TF-F004 Implementation Capture - Credential Domain Model

## Runtime Changes

- Added `src/security/__init__.py`.
- Added `src/security/credential.py`.
- Added focused tests in `tests/test_credential.py`.
- Updated runtime operational state in `DOCS/ISSUE_REGISTER.md` and `DOCS/Milestone_Roadmap_v2.md`.

## Resulting Model

- `CredentialStatus`: `active`, `revoked`, `expired`, `unknown`
- `Credential`: immutable credential metadata with provider identity, credential shape, encrypted payload, lifecycle timestamps, status, and provenance.
- Validation added for required values and timestamp ordering.
- Provenance is copied into an immutable mapping at construction time.

## Event / Replay / Lifecycle Notes

- No canonical events added.
- No lifecycle semantics changed.
- Replay-related fields now exist in the runtime model so later credential-history work can build on a stable shape.

## Verification

- `uv run pytest tests\\test_credential.py` -> 7 passed
- `uv run ruff check src\\security tests\\test_credential.py` -> clean

## Deferred Work

- TF-F005: encryption and local store implementation
- TF-F006: composition-root credential wiring
- TF-F007: operator setup and rotation documentation

## Observations

- ADR-0037 was already accepted before implementation, so TF-F004 became a bounded realization issue rather than an architectural discovery issue.
- `STARTUP.md` remains absent from the KB path referenced by bootstrap/runtime loading guidance.
