# TradeForge Codex Prompt — Build Vertical Slice

You are Codex working inside the TradeForge runtime repository.

Your task is to build a narrow, correct vertical slice of TradeForge while preserving the canonical decision lifecycle and event-sourced architecture.

## Required Context

Before coding, read:

- `C:\Users\bosto\dockerstuff\knowledge-base\TradeForge\STARTUP.md`
- `C:\Users\bosto\dockerstuff\knowledge-base\TradeForge\INVARIANTS.md`
- `C:\Users\bosto\dockerstuff\knowledge-base\TradeForge\ARCHITECTURE.md`
- `C:\Users\bosto\dockerstuff\knowledge-base\TradeForge\GLOSSARY.md`
- `C:\Users\bosto\dockerstuff\knowledge-base\TradeForge\WORKSPACES.md`
- `TradeForge/AGENTS.md`
- relevant ADRs under `TradeForge/docs/adr/`

## Core Rule

Build only the requested vertical slice.

Do not broaden scope.

Do not create generic infrastructure unless the slice requires it.

Do not introduce broker automation, dashboards, prediction engines, or AI authority.

## TradeForge Lifecycle

The canonical lifecycle is:

Idea → Thesis → Plan → Approval → Execution → Position → Review


You must not collapse these stages.
Each stage must have distinct meaning.
Event-Sourcing Rules


Commands request change.


Events record accepted facts.


Current state is derived from events.


No durable state may bypass the event stream.


Replay must reconstruct lifecycle state.


Events must be named in domain language.


Implementation Requirements
For the requested slice:


Define the command or user action.


Define the event or events produced.


Define the aggregate or lifecycle state affected.


Define replay behavior.


Define validation rules.


Define tests proving replay and invariant preservation.


Update docs only when the slice introduces new architectural meaning.


Required Boundaries
Do not treat UI screens as architecture.
Do not treat AI suggestions as decisions.
Do not allow execution without approval.
Do not allow position state without execution facts.
Do not allow review before position closure.
Do not store mutable lifecycle truth outside events.
Testing Requirements
Add tests that prove:


valid command produces expected event


invalid command is rejected


replay reconstructs expected state


lifecycle order is enforced


advisory inputs do not become authoritative decisions


Output
Implement the smallest complete slice.
At completion, report:


files changed


commands added


events added


lifecycle stage affected


invariants protected


tests added


commands to run tests


deferred work


