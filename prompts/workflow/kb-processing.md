---
title: KB Processing Workflow Prompt
type: workflow-prompt
status: canonical
tags:
  - TradeForge
  - kb-processing
  - semantic-stabilization
  - ontology
  - replayability
  - workflow
created: 2026-05-08
updated: 2026-05-09
prompt_scope: knowledge-base-processing
related:
  - [[Runtime ↔ KB Development Loop]]
  - [[SEMANTIC_GOVERNANCE]]
  - [[ARCHITECTURE]]
  - [[EVENT_TAXONOMY]]
  - [[EXECUTION_CONTRACT]]
  - [[INVARIANTS]]
---

# KB Processing Workflow Prompt

## Purpose

This prompt operationalizes KB semantic stabilization behavior for TradeForge.

It is used when processing:
- raw planning notes
- implementation captures
- architecture reasoning
- workflow observations
- replay analysis
- milestone reflections
- ontology discoveries

The purpose is to transform unstable knowledge into progressively stabilized semantic structures.

This prompt exists to prevent:
- passive raw-note accumulation
- ontology drift
- terminology inconsistency
- architectural memory loss
- replayability degradation
- disconnected reasoning artifacts

---

# Core Principle

The KB is NOT:
- passive note storage
- generic documentation
- disconnected markdown files

The KB IS:
- semantic governance
- replayable cognition
- ontology stabilization
- architectural memory
- reusable operational reasoning

Knowledge should evolve progressively:

```text
raw discovery
    ↓
processed synthesis
    ↓
topic refinement
    ↓
canonical entity definition
    ↓
architectural stabilization
```

---

# Required Semantic Initialization

Before processing knowledge, load semantic context.

Required:
- [[SEMANTIC_BOOTSTRAP]]
- [[SEMANTIC_GOVERNANCE]]
- [[INVARIANTS]]
- [[ARCHITECTURE]]
- [[GLOSSARY]]
- [[EVENT_TAXONOMY]]
- [[EXECUTION_CONTRACT]]

Optional depending on task:
- [[PERSONAS]]
- [[WORKSPACES]]
- relevant ADRs
- relevant workflows
- relevant playbooks

If semantic initialization has not occurred:
STOP.

---

# Processing Responsibilities

When processing a raw knowledge capture, perform ALL applicable analysis.

---

# 0. Normalize Messy Notes When Needed

If the raw capture is messy, unstructured, pasted from chat, or primarily feedback/friction rather than already structured architecture knowledge, Codex MAY first use the local skill:

```text
skills/ontology/structure-feedback-note.md
```

Use this skill to convert the messy input into a structured raw feedback note before semantic promotion.

Rules:
- preserve the original thinking under a raw input or source section
- do not treat structured feedback as canonical knowledge
- do not promote directly from a messy note if important context would be lost
- after structuring, continue KB processing from the structured note when promotion is still requested
- if the skill moves or preserves the original source, record that source relationship in the processing output

This step is a normalization aid, not a replacement for semantic analysis.

---

## Deep Integration Heuristics

For architectural, workflow, doctrine, ontology,
or operational cognition notes, additionally evaluate:

- ontology implications
- workflow implications
- playbook implications
- doctrine integration opportunities
- cross-linking opportunities
- invariant conflicts
- semantic overlap risks
- canonicalization readiness
- replayability implications
- architectural synchronization impacts

Codex should:
- avoid premature canonicalization
- avoid semantic duplication
- identify convergence opportunities
- preserve ontology stability
- surface doctrine conflicts explicitly

Not every note requires deep integration analysis.

Use stronger synthesis behavior when:
- architectural meaning is emerging
- recurring concepts appear
- operational patterns stabilize
- workflow cognition evolves
- semantic boundaries shift
- canonical structure may require refinement

---


# 1. Detect Semantic Implications

Identify:
- important concepts
- durable terminology
- architectural meanings
- recurring operational ideas

Questions:
- Does this introduce important terminology?
- Does this redefine existing concepts?
- Does this affect semantic consistency?
- Does this reveal hidden assumptions?

---

# 2. Detect Ontology Implications

Identify:
- new entities
- new relationships
- new lifecycle concepts
- new bounded contexts
- new semantic structures

Questions:
- Does this belong in `knowledge/entities/`?
- Does this affect ontology structure?
- Does this require cross-linking?
- Does this conflict with canonical definitions?

Call out ontology drift explicitly.

Never silently mutate core semantics.

---

# 3. Detect Workflow Implications

Identify:
- recurring operational sequences
- repeatable execution patterns
- reusable development flows
- replay procedures

Questions:
- Does this define a reusable process?
- Should this become a workflow?
- Should an existing workflow be updated?
- Does this reveal missing operational governance?

---

# 4. Detect Playbook Implications

Identify:
- governance failures
- repeatable failure modes
- missing operational rules
- architectural discipline gaps

Questions:
- Does this reveal a missing checklist item?
- Does this reveal stabilization drift?
- Does this require a new operational rule?
- Does this require playbook updates?

---

# 5. Detect ADR Implications

Identify:
- architecture tradeoffs
- future-facing design decisions
- semantic boundary decisions
- replayability implications

Questions:
- Should a runtime ADR exist?
- Does an existing ADR require updates?
- Was architectural reasoning left undocumented?

If architectural implications exist:
surface them explicitly.

---

# 6. Detect Operational Synchronization Issues

Verify alignment between:
- runtime repo state
- issue register
- roadmap
- KB references
- branch naming
- implementation reality

Questions:
- Is roadmap state stale?
- Is issue state stale?
- Does branch history conflict with issue tracking?
- Is milestone state inconsistent?
- Are KB indexes outdated?

Operational drift must be surfaced explicitly.

---

# 7. Detect Terminology Drift

Verify:
- glossary consistency
- ontology consistency
- canonical naming alignment
- event naming discipline

Avoid:
- alternate terminology
- inconsistent naming
- accidental semantic mutation

When terminology evolves:
- document explicitly
- preserve historical references
- note deprecated terms

---

# 8. Promote Stable Knowledge

Promote knowledge ONLY when confidence is sufficiently high.

Promotion targets:

| Knowledge Type | Target |
|---|---|
| exploratory reasoning | `knowledge/processed/` |
| recurring themes | `knowledge/topics/` |
| canonical concepts | `knowledge/entities/` |
| operational governance | `playbooks/` |
| executable procedures | `workflows/` |
| reusable invocation surfaces | `prompts/` |
| reusable reasoning capability | `skills/` |

Rules:
- prefer incremental refinement
- avoid premature canonicalization
- preserve replayable reasoning
- preserve rejected alternatives when useful

---

# 9. Update Canonical Structures

When justified, update:
- ontology references
- workflows
- playbooks
- prompts
- skills
- topic pages
- entity pages
- KB indexes

The KB is an evolving semantic system.

Stable knowledge SHOULD update canonical structures.

Passive accumulation is discouraged.

---

# 10. Preserve Replayability

Preserve:
- reasoning context
- architecture tradeoffs
- unresolved concerns
- milestone context
- implementation assumptions
- semantic evolution history

Do NOT:
- silently erase prior reasoning
- collapse historical context
- flatten semantic evolution

Replayability is foundational.

---

# 11. Delete Successfully Processed Raw Capture

At the end of a successful processing pass, delete the source raw note from:

```text
knowledge/raw/
```

Deletion is allowed ONLY when all of the following are true:

- stable content has been promoted to the appropriate processed, topic, entity, workflow, playbook, prompt, skill, or index location
- the promoted artifact records the source raw note path or title
- unresolved questions, rejected alternatives, contradictions, and operational findings have been preserved in the promoted artifact or explicitly deferred
- no required semantic, ontology, workflow, playbook, ADR, or operational synchronization action remains incomplete
- the processing result can be reconstructed from the promoted artifacts and git history

If processing is incomplete, ambiguous, blocked, or only partially successful, do NOT delete the raw note.

Rules:
- raw note deletion must be explicit, not silent
- never delete raw notes before promotion and traceability are complete
- never delete raw notes to hide uncertainty, contradictions, or unresolved architectural concerns
- mention the deleted raw note in the processing result

This rule prevents passive raw-note accumulation while preserving replayability through stabilized artifacts.

---

# Mutation Policy

## Automatically Allowed

Codex MAY:
- refine terminology
- create processed syntheses
- delete successfully processed raw notes after traceable promotion
- update topic pages
- update workflow docs
- update playbooks
- update prompts
- update KB indexes
- add semantic cross-links
- improve ontology references

---

## Requires Explicit Caution

Codex should be conservative when:
- changing core invariants
- redefining ontology structures
- changing lifecycle semantics
- changing event taxonomy
- mutating canonical doctrine
- replacing historical terminology

---

## Never Do Automatically

Codex MUST NOT:
- silently delete historical reasoning
- delete unprocessed or partially processed raw notes
- silently replace ontology definitions
- bypass invariants
- rewrite architectural doctrine without justification
- fabricate semantic certainty

---

# Processing Output Expectations

Processing should ideally produce:

- note normalization result, when `structure-feedback-note` was used
- semantic synthesis
- ontology observations
- stabilization recommendations
- workflow implications
- playbook implications
- ADR implications
- operational synchronization findings
- suggested promotions
- deleted raw note path, when processing completed successfully
- identified contradictions

---

# Anti-Patterns

Avoid:
- passive note archival
- disconnected markdown sprawl
- giant monolithic documents
- ontology explosion
- semantic duplication
- silent terminology drift
- premature canonicalization
- unexplained architecture mutation

---

# Final Principle

TradeForge knowledge processing exists to preserve:

- semantic continuity
- replayable reasoning
- ontology stability
- workflow integrity
- architectural memory
- long-term cognitive coherence

The goal is NOT merely storing information.

The goal is stabilizing durable operational cognition.
