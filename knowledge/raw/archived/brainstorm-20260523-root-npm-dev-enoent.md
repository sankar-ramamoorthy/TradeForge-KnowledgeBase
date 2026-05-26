---
title: Root npm run dev ENOENT
type: brainstorm
status: raw
tags: [tradeforge, brainstorm, npm, frontend, developer-experience, bug]
created: 2026-05-23
---

# Root npm run dev ENOENT

## Trigger

Operator attempted to start the frontend from the TradeForge repository root.

## Raw Input

```text
Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows

PS C:\Users\bosto\dockerstuff\TradeForge> npm run dev
npm error code ENOENT
npm error syscall open
npm error path C:\Users\bosto\dockerstuff\TradeForge\package.json
npm error errno -4058
npm error enoent Could not read package.json: Error: ENOENT: no such file or directory, open 'C:\Users\bosto\dockerstuff\TradeForge\package.json'
npm error enoent This is related to npm not being able to find a file.
npm error enoent
npm error A complete log of this run can be found in: C:\Users\bosto\AppData\Local\npm-cache\_logs\2026-05-23T16_59_11_953Z-debug-0.log
PS C:\Users\bosto\dockerstuff\TradeForge>
```

## Observations

- The operator ran `npm run dev` from the repository root.
- The frontend package lives under `frontend/`.
- The root repository has no `package.json`, so npm fails before it can provide a useful project-specific hint.
- The README documents `cd frontend` before `npm run dev`, but this is easy to miss during routine operation.

## Ideas

- Add a root `package.json` with script shims that delegate to `frontend/`.
- Preserve the frontend package as the owner of React/Vite dependencies.
- Keep backend Python tooling unchanged.
- Add a focused test that verifies the root npm scripts exist and delegate to `frontend`.

## Questions

- Should root npm scripts include only `dev`, or also `typecheck`, `build`, and `lint` for consistency?
- Should README continue to show `cd frontend`, or should it advertise root-level commands?

## Concerns

- Duplicating frontend dependencies at the repo root would blur runtime boundaries.
- Moving the frontend package would create unnecessary churn.
- Root scripts must remain thin operational conveniences, not a new frontend authority.

## Possible Next Outputs

- Issue candidate.
- Diagnosis note.
- Planning note.
- Runtime fix.
