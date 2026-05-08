# TradeForge Codex Prompt — Replay Analysis

You are Codex working inside the TradeForge runtime repository.

Your task is to analyze replay behavior in TradeForge and verify that lifecycle state can be reconstructed from events without architectural drift.

## Required Context

Before analysis, read:

- `C:\Users\bosto\dockerstuff\knowledge-base\TradeForge\STARTUP.md`
- `C:\Users\bosto\dockerstuff\knowledge-base\TradeForge\INVARIANTS.md`
- `C:\Users\bosto\dockerstuff\knowledge-base\TradeForge\ARCHITECTURE.md`
- `C:\Users\bosto\dockerstuff\knowledge-base\TradeForge\GLOSSARY.md`
- `TradeForge/AGENTS.md`
- relevant ADRs under `TradeForge/docs/adr/`
- event-related code under `TradeForge/src/`

## Mission

Determine whether TradeForge can reconstruct meaningful decision lifecycle state from recorded events.

Replay analysis must focus on semantic correctness, not just technical event loading.

## Canonical Lifecycle


Idea → Thesis → Plan → Approval → Execution → Position → Review

Replay must preserve this sequence.

Analysis Checklist

For each event type, identify:

event name
lifecycle stage
source command
payload meaning
aggregate affected
state derived from it
invariants enforced
invalid prior states
valid next states
Required Questions

Answer:

Can the event stream reconstruct current lifecycle state?
Are there any hidden mutable state assumptions?
Are any lifecycle stages collapsed?
Can replay distinguish intent from execution fact?
Can replay distinguish advisory AI output from accepted user decision?
Can replay detect invalid transitions?
Can replay support future review and learning workflows?
Drift Detection

Flag any sign of:

dashboard-centric state
mutable status fields acting as source of truth
AI output treated as decision
execution before approval
position state without execution facts
review before lifecycle completion
missing event names
ambiguous event payloads
non-replayable side effects
Output Format

Produce a Markdown report with:
# Replay Analysis Report

## Summary

## Event Inventory

## Lifecycle Reconstruction

## Invariants Verified

## Drift Risks

## Replay Gaps

## Recommended Fixes

## Tests Needed

## ADRs Needed

Constraints

Do not modify code unless explicitly instructed.

Do not introduce new architecture casually.

If fixes are needed, recommend them as issues or ADR candidates.

Preserve TradeForge’s event-sourced decision lifecycle.