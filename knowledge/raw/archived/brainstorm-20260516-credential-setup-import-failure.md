---
title: Credential Setup Command Fails To Import src Module
type: brainstorm
status: raw
tags: [tradeforge, brainstorm, credential-setup, operator-friction, packaging]
created: 2026-05-16
---

# Credential Setup Command Fails To Import src Module

## Trigger

Operator followed `HOW-TO-SETUP-KEYS.md` to generate the TradeForge master key from the repository root.

## Raw Input

```powershell
PS C:\Users\bosto\dockerstuff\TradeForge> uv run python scripts\manage_credentials.py generate-master-key
Traceback (most recent call last):
  File "C:\Users\bosto\dockerstuff\TradeForge\scripts\manage_credentials.py", line 7, in <module>
    from src.security import Credential, CredentialStatus, CredentialStore, KeyManager
ModuleNotFoundError: No module named 'src'
PS C:\Users\bosto\dockerstuff\TradeForge>
```

## Observations

- The documented credential setup command fails when executed exactly as written from the repository root.
- `scripts\manage_credentials.py` imports `src.security`, but the runtime invocation path does not make `src` importable in this execution mode.
- This blocks first-time provider credential setup before the operator can configure `fmp` or `alpha_vantage`.
- The issue is operationally significant because M10C documentation claims credential setup can be completed quickly through the guide.

## Ideas

- Re-evaluate whether the credential script should be invoked as a module, exposed through a project command, or made path-safe when run directly.
- Update the guide after the command path is corrected so the first documented setup step works verbatim.
- Add regression coverage for the documented CLI invocation path, not only for internal credential-store behavior.

## Questions

- Should the supported command be `uv run python -m scripts.manage_credentials ...`, a console script entry point, or a direct script path with explicit path bootstrapping?
- Should the setup guide prefer one canonical invocation form for all credential commands?

## Concerns

- Credential setup friction blocks provider onboarding before any provider-layer feature can be exercised.
- Documentation that cannot be followed verbatim weakens operator trust in the credential boundary.
- If script invocation is not standardized now, future operational utilities may repeat the same packaging/import problem.

## Possible Next Outputs

- Feedback issue candidate
- Credential setup documentation correction
- CLI invocation regression test
