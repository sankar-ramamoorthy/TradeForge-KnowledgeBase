Absolutely. This is exactly the point where a proper design document is worth doing before implementation.

And I agree with your instinct:
the import experience must feel coherent across:

* Thesis
* Plan
* Review
* later behavioral/reflection workspaces

otherwise the UX becomes fragmented and cognitively inconsistent.

I would frame this as a cross-workspace interaction doctrine, not merely a file import feature.

The key idea becomes:

> External research may assist lifecycle cognition, but lifecycle intent, authorization, and canonicalization remain explicitly operator-controlled.

The design document should therefore cover:

* philosophy
* workflow
* cognitive boundaries
* UX patterns
* provenance
* replay semantics
* workspace consistency
* event implications
* future extensibility

I would structure the document roughly like this:

---

# Proposed Document

## ADR or Design Spec?

I would NOT make this an ADR yet.

Reason:
you are still exploring interaction shape.

Better:

* design spec first
* thin implementation
* operational testing
* then ADR once stabilized

So something like:

```text
DOCS/design/M14C_RESEARCH_IMPORT_BRIDGE.md
```

or in KB:

```text
knowledge/raw/research-import-bridge-design.md
```

before promotion.

---

# Recommended Sections

## 1. Purpose

Why this exists.

Key idea:

TradeForge is not intended to be the primary long-form writing environment.

Operators increasingly use:

* ChatGPT
* Claude
* Codex
* research notes
* markdown documents
* external AI tooling

TradeForge must ingest structured reasoning without surrendering canonical authority.

---

## 2. Core Principles

This section is critical.

Examples:

### Human Decision Sovereignty

Imported material is advisory until explicitly accepted by the operator.

### Lifecycle Integrity

Imports never bypass lifecycle transitions.

### Canonical Boundary

External artifacts never directly emit canonical lifecycle events.

### Replayability

Imported provenance must remain replay-visible.

### Cognitive Transparency

The operator must know:

* what was imported
* from where
* what was modified
* what became canonical

---

## 3. UX Philosophy

This is where your hybrid approach gets formalized.

Example:

### Preserve Existing Lifecycle Entry

The operator still explicitly enters Thesis/Plan/etc.

### Import Assists Authorship

Importing supports reasoning construction but does not replace it.

### Selective Acceptance

Operators may accept:

* individual fields
* grouped suggestions
* entire sections

### Manual Risk Ownership

Certain fields remain operator-authored:

* conviction
* sizing
* approval
* authorization
* execution authority

---

# 4. Cross-Workspace Interaction Pattern

This is the important part you just identified.

You want the SAME mental model everywhere.

Example structure:

| Workspace  | Import Capability               | Canonical Boundary    |
| ---------- | ------------------------------- | --------------------- |
| Thesis     | narrative/catalysts/assumptions | manual promotion      |
| Plan       | entries/targets/risks           | manual validation     |
| Review     | reflections/lessons             | operator confirmation |
| Replay     | advisory overlays only          | immutable history     |
| Behavioral | non-canonical interpretation    | no auto-judgement     |

This section may become one of the most important UX doctrine sections in the entire system.

---

# 5. Thesis Workspace Import Design

Document:

* global import
* preview state
* per-field accept
* provenance markers
* editing flow

You can even include wireframes.

---

# 6. Plan Workspace Import Design

This one matters even more.

Because importing plans is dangerous.

You probably want:

* imported entries
* imported targets
* imported invalidation ideas

BUT:

* sizing
* approval
* execution authorization

should remain explicitly operator-entered.

---

# 7. Review Workspace Import Design

Very important long-term.

Example:

* import journal reflections
* import AI-generated review suggestions
* import behavioral observations

But:

* operator confirms lessons learned

This becomes foundational for future M14/M15 cognition.

---

# 8. Provenance Model

Critical section.

Imported material should preserve:

* source file
* import timestamp
* source system
* optional source model/provider
* operator edits after import

This will matter enormously later for replay and cognitive auditability.

---

# 9. Event Model Considerations

Examples:

Non-canonical:

* ResearchDraftImported
* ImportedFieldAccepted
* ImportedDraftRejected

Canonical only after promotion:

* ThesisCreated
* PlanStructured
* ReviewRecorded

---

# 10. Future Extensions

This is where you mention:

* Research Cockpit integration
* AI-generated trade candidates
* advisor feed ingestion
* watchlist imports
* thesis evolution imports
* structured markdown contracts
* local filesystem watchers
* eventual MCP ingestion

---

# Strong suggestion

You should probably define a canonical external artifact schema early.

Something like:

```yaml
artifact_type: thesis_draft
symbol: ATKR
source: claude_code
generated_at: ...
thesis:
catalysts:
assumptions:
invalidation:
```

Even if primitive initially.

Because once imports start existing, schema drift becomes painful very fast.

---

This is one of those areas where careful UX and architectural discipline could genuinely differentiate TradeForge from:

* generic AI copilots
* journaling apps
* automated trading assistants

because you are explicitly preserving:

* cognition provenance
* replayability
* operator accountability
* canonical boundaries

while still embracing AI-assisted workflows.
