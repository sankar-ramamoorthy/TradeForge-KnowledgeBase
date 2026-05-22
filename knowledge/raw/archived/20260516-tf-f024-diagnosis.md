---
title: TF-F024 Diagnosis - Credential Script Direct Invocation Import Failure
type: brainstorm
status: raw
tags: [tradeforge, tf-f024, diagnosis, credential-setup]
created: 2026-05-16
---

# TF-F024 Diagnosis - Credential Script Direct Invocation Import Failure

## Observable Gap

Running the documented command from the repository root fails:

```powershell
uv run python scripts\manage_credentials.py generate-master-key
```

The script exits before doing useful work with `ModuleNotFoundError: No module named 'src'`.

## Root Cause

When Python executes a file by path, it places the script directory (`scripts/`) on `sys.path`, not the repository root. `manage_credentials.py` imports `src.security`, so direct execution from the documented path cannot resolve the package unless the repository root is added explicitly or the invocation model changes.

## Relevant Invariants

- Architectural Simplicity: the credential setup path should remain explicit and easy to follow.

## Minimum Correct Scope

Make the documented direct script invocation work verbatim and add regression coverage for that invocation style.

## Out Of Scope

- redesigning the credential tool into a package CLI
- changing credential semantics
- unrelated provider setup work
