---
title: Docker Compose Runtime Does Not See Host Master Key
type: brainstorm
status: raw
tags: [tradeforge, brainstorm, credential-setup, docker-compose, operator-friction]
created: 2026-05-17
---

# Docker Compose Runtime Does Not See Host Master Key

## Trigger

Operator started TradeForge through Docker Compose after configuring the credential master key in PowerShell.

## Raw Input

```powershell
tradeforge-1  | src.security.key_manager.MasterKeyNotConfiguredError: TRADEFORGE_MASTER_KEY is required in the OS environment
tradeforge-1 exited with code 1 (restarting)
tradeforge-1  | Bytecode compiled 4736 files in 292ms
Gracefully Stopping... press Ctrl+C again to force
Container tradeforge-tradeforge-1 Stopping w Enable Watch   d Detach
Container tradeforge-tradeforge-1 Stopped
Container tradeforge-postgres-1 Stopping
Container tradeforge-postgres-1 Stopped

PS C:\Users\bosto\dockerstuff\TradeForge> $env:TRADEFORGE_MASTER_KEY
oJDcuSqz0yl4L_pscCgrW2Lq0HrFx2AS3CBrjUhOztM=
PS C:\Users\bosto\dockerstuff\TradeForge>
```

The operator noted that they intend to rotate the master key, but the immediate concern is that the configured PowerShell value is not being found by the running service.

## Observations

- The host PowerShell session contains `TRADEFORGE_MASTER_KEY`.
- The runtime container still exits as if the variable is missing.
- This suggests a boundary issue between the host shell and the Docker service environment rather than a missing host configuration.
- The failure blocks credentialed-provider startup before normal runtime behavior begins.

## Ideas

- Check whether `docker-compose.yml` forwards `TRADEFORGE_MASTER_KEY` into the `tradeforge` service.
- Prefer a fail-fast Compose guard so missing host configuration is reported before the app starts.
- Document the Compose-specific requirement in the credential setup guide.

## Questions

- Should Compose require the key only when credentialed providers are configured, or always while the credential store exists?
- Is any separate provider-selection variable also expected to be forwarded by Compose?

## Concerns

- Operator-visible configuration that exists on the host but not in the container is easy to misdiagnose.
- A runtime restart loop obscures the true cause more than a Compose-time configuration error would.
- Secret handling should remain outside `.env` and Git while still being explicit at the container boundary.

## Possible Next Outputs

- Feedback issue candidate
- Docker Compose configuration fix
- Credential setup documentation update
