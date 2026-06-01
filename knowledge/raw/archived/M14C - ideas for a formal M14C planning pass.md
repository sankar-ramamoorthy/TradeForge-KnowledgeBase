# ideas for a formal M14C planning pass
Next I would do a **formal M14C planning pass** before any implementation.

Specifically:

---

# 1. Create the milestone definition

Not code yet.

Define:

```text
M14C — Research Cockpit Import Bridge
```

This becomes the umbrella milestone between:

* M14 Behavioral Intelligence
  and
* M15 future cognition/workflow expansion.

Purpose statement:

> Introduce controlled ingestion of external research artifacts into TradeForge while preserving lifecycle discipline, canonical boundaries, replayability, and explicit operator approval.

---

# 2. Define the first thin vertical slice

This is critical.

Do not build “universal import.”

Build ONE clean path end-to-end.

I would strongly recommend:

## TF-R001 — Thesis Draft Import

Scope:

```text
filesystem markdown
→ parsed research artifact
→ thesis workspace import preview
→ selective field acceptance
→ manual thesis creation
→ replay provenance
```

Nothing more.

No watcher daemon.
No background sync.
No AI parsing.
No plan import.
No execution import.

Just:

* deterministic
* visible
* operator-controlled
* replayable

---

# 3. Define the canonical import schema

This is probably the most important technical design decision next.

Before UI implementation.

You need a stable external artifact contract.

Example v1:

```yaml
artifact_type: thesis_draft
schema_version: 1

symbol: ATKR
source:
  system: claude_code
  model: claude-sonnet-4
  generated_at: 2026-05-26T18:00:00Z

thesis:
  narrative: >
    Aerospace recovery and defense spending...
    
catalysts:
  - earnings acceleration
  - institutional accumulation

assumptions:
  - market remains risk-on

invalidation:
  - breaks below 200-day moving average

notes:
  - generated from external research cockpit
```

That schema becomes foundational later.

Without this, imports will become chaotic very quickly.

---

# 4. Design the import state model

You need explicit lifecycle semantics for imported artifacts.

Likely:

```text
Detected
Parsed
Previewed
Accepted
Rejected
Promoted
Archived
```

And importantly:

```text
Imported ≠ Canonical
```

must remain true everywhere.

---

# 5. Design the thesis workspace interaction flow

This should probably be the next concrete deliverable after the design doc.

Meaning:

* annotated wireframes
* interaction sequences
* import preview behavior
* provenance indicators
* field acceptance behavior

This is where your hybrid UX gets finalized.

---

# 6. Decide where imported artifacts live

This matters architecturally.

Options:

## A. Filesystem-only

Simplest.

```text
/imports/incoming
/imports/processed
/imports/rejected
```

Good for first slice.

---

## B. Event-backed advisory artifacts

Better long-term.

Meaning:

* imported artifacts become replay-visible entities
* import actions emit events
* provenance becomes first-class

I suspect TradeForge eventually wants this.

But for TF-R001:
filesystem + replay metadata is enough.

---

# 7. Define prohibited behaviors early

This is VERY important.

Explicitly forbid:

* auto thesis creation
* auto plan creation
* auto approval
* auto conviction scoring
* auto sizing
* auto execution authorization

Those boundaries should be doctrinally explicit.

---

# 8. Only after all that:

## create implementation issues

Likely sequence:

```text
TF-R001 — Thesis draft filesystem import
TF-R002 — Thesis import preview UX
TF-R003 — Imported provenance overlays
TF-R004 — Replay visibility for imports
TF-R005 — Plan workspace import support
```

---

This is one of the few areas where slowing down for design first is absolutely the correct move.

Because once users start importing AI-generated cognition into lifecycle systems, the boundary mistakes become extremely difficult to undo later.
