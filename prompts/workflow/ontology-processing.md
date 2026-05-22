---
title: Ontology Processing Workflow Prompt
type: workflow-prompt
status: canonical
tags:
  - TradeForge
  - ontology-processing
  - semantic-governance
  - canonical-readiness
  - concept-modeling
  - terminology-discipline
  - replayability
created: 2026-05-21
updated: 2026-05-21
prompt_scope: ontology-processing
governs:
  - ontology promotion
  - concept relationship modeling
  - canonical semantic structure
  - terminology stabilization
  - cross-entity relationship maps
related:
  - [[SEMANTIC_BOOTSTRAP]]
  - [[SEMANTIC_GOVERNANCE]]
  - [[INVARIANTS]]
  - [[GLOSSARY]]
  - [[ARCHITECTURE]]
  - [[kb-processing]]
---

# Ontology Processing Workflow Prompt

## Purpose

This prompt governs when and how TradeForge writes to `ontology/`.

Ontology processing is used when a concept cluster has become stable enough to
need formal relationship modeling across entities, topics, workflows, doctrine,
or runtime architecture.

This prompt exists to prevent:

- premature ontology creation
- duplicate entity/topic pages
- terminology drift
- over-formalized implementation details
- disconnected semantic maps
- silent changes to domain meaning
- ontology files that merely summarize processed notes

`ontology/` is the highest semantic modeling layer in the KB. It should change
only when the system's conceptual structure needs explicit representation.

---

# Core Principle

Do NOT write to `ontology/` just because a concept is important.

Write to `ontology/` only when the relationships between concepts are stable,
recurring, and architecturally meaningful enough to require a durable semantic
model.

The rough maturity path is:

```text
raw discovery
    -> processed synthesis
    -> topic refinement
    -> entity stabilization
    -> ontology relationship modeling
```

Ontology is not a replacement for `processed/`, `topics/`, or `entities/`.
Ontology explains how stable concepts relate.

---

# Invocation

Use this prompt when the user explicitly asks for ontology processing, ontology
promotion, or formal concept modeling.

Suggested invocation:

```text
Use prompts/workflow/ontology-processing.md.

Evaluate whether this concept cluster is ready for ontology promotion.

Do not create ontology files unless canonical readiness is satisfied.
```

---

# Required Semantic Initialization

Before ontology processing, load:

- `SEMANTIC_BOOTSTRAP.md`
- `INVARIANTS.md`
- `SEMANTIC_GOVERNANCE.md`
- `knowledge/index/runtime-context-map.md`
- `GLOSSARY.md`
- `ARCHITECTURE.md`

Load task-specific context after that:

- relevant `knowledge/entities/` pages
- relevant `knowledge/topics/` pages
- relevant `knowledge/processed/` syntheses
- relevant ADRs
- relevant workflow docs
- relevant doctrine files, only if the concept cluster touches them

Avoid loading the whole KB by default.

If semantic initialization has not occurred:

```text
STOP.
```

---

# Ontology Readiness Criteria

Ontology promotion is justified only when most of these are true:

- the concept cluster recurs across multiple processed notes, topics, issues, or milestones
- the relationships between concepts matter more than one concept alone
- existing `entities/` pages need a shared structural map
- terminology drift risk exists without a formal relationship model
- the model affects architecture, workflow, replay, UX, AI governance, or source-of-truth boundaries
- the concept cluster has clear boundaries against neighboring concepts
- contradictions or unresolved alternatives have been identified and preserved
- the ontology model can be stated without inventing speculative future architecture

If readiness is partial, keep the work in `topics/` or `processed/`.

---

# Valid Ontology Targets

Ontology files may define:

- concept taxonomies
- relationship models
- bounded-context maps
- source-of-truth distinctions
- lifecycle/state relationship structures
- authority boundaries
- provenance and replay relationship structures
- advisory vs canonical vs derived vs inferred state models
- persona/workspace/decision relationships
- terminology evolution maps

Ontology files should NOT be:

- issue summaries
- implementation plans
- runtime class documentation
- broad doctrine essays
- copied processed notes
- generic architecture overviews
- speculative future feature maps

---

# Processing Sequence

## 1. Identify Concept Cluster

Name the cluster being evaluated.

Examples:

- advisory context and provenance
- provider capability and external-data boundaries
- workspace cognition and decision surfaces
- replay reconstruction and historical snapshots
- decision artifacts and lifecycle authority

Clarify whether the request concerns:

- a new ontology file
- an update to an existing ontology file
- a recommendation not to promote yet

---

## 2. Gather Existing Semantic Material

Inspect existing material in this order:

1. `INVARIANTS.md`
2. `GLOSSARY.md`
3. relevant `knowledge/entities/`
4. relevant `knowledge/topics/`
5. relevant `knowledge/processed/`
6. relevant ADRs and doctrine
7. raw notes only if processed material is insufficient

Prefer stable summaries over raw notes.

Do not treat raw notes or generated outputs as canonical truth.

---

## 3. Detect Existing Authority

Determine whether the concept is already governed by:

- invariants
- glossary definitions
- canonical entity pages
- doctrine files
- ADRs
- runtime docs

If existing authority is sufficient, do not create ontology.

If existing authority conflicts, report the contradiction before editing.

---

## 4. Define Relationship Model

If ontology promotion is justified, define:

- concepts in the model
- relationships between concepts
- ownership and authority boundaries
- canonical vs derived vs inferred vs advisory distinctions
- lifecycle implications
- event/replay implications
- terminology boundaries
- concepts explicitly out of scope

Prefer compact relationship maps.

Example:

```text
Provider Output
    -> normalized into Market Context
    -> interpreted into Advisory Artifact
    -> linked as Cognitive Evidence
    -> surfaced in Persona Workspace
    -> may inform Decision Lifecycle
    -> never mutates Event Ledger directly
```

---

## 5. Validate Against Invariants

Before writing ontology, verify:

- human decision sovereignty is preserved
- Event Ledger authority is preserved
- lifecycle authority is not reassigned
- replay does not depend on live external state
- AI/advisory systems remain non-authoritative
- derived and inferred state remain distinguishable
- terminology remains consistent with `GLOSSARY.md`

If any invariant is weakened:

```text
STOP.
```

---

## 6. Choose Promotion Target

Select the correct target:

| Material | Target |
|---|---|
| single issue synthesis | `knowledge/processed/` |
| recurring theme | `knowledge/topics/` |
| durable canonical concept | `knowledge/entities/` |
| formal relationship model | `ontology/` |
| system-wide rule | doctrine or invariant review |
| architecture decision | ADR |
| operational procedure | `workflows/` or `prompts/` |

Do not promote directly to ontology if a topic or entity page is the better fit.

---

## 7. Write Ontology Artifact

If writing is justified, ontology files should include YAML front matter.

Recommended front matter:

```yaml
---
title: Example Ontology
type: ontology
status: draft
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [TradeForge, ontology]
governs:
  - concept relationships
related:
  - [[Relevant Entity]]
---
```

Recommended body sections:

```markdown
# Example Ontology

## Purpose
## Scope
## Concepts
## Relationship Model
## Authority Boundaries
## Lifecycle Implications
## Event And Replay Implications
## Terminology Boundaries
## Out Of Scope
## Open Questions
```

Keep the ontology concise and structural.

---

## 8. Update References

When ontology is created or materially changed, update relevant backlinks or
indexes where appropriate:

- related entity pages
- related topic pages
- relevant processed syntheses
- `knowledge/index/` files, if an index exists for the concept area
- `GLOSSARY.md`, only when terminology becomes canonical

Do not update broad doctrine unless the ontology changes doctrine.

---

## 9. Preserve Replayability

Ontology changes must preserve:

- source materials used
- promotion rationale
- unresolved questions
- rejected alternatives
- contradictions found
- why ontology was chosen over topic/entity promotion

Do not erase earlier processed reasoning.

---

# Required Output Format

Ontology processing output SHOULD include:

```markdown
# Ontology Processing Result

## Concept Cluster
## Source Material Reviewed
## Existing Authority
## Readiness Assessment
## Recommended Target
## Relationship Model
## Invariant Check
## Ontology Changes
## Reference Updates
## Open Questions
## Final Status
```

If ontology is not created, say so explicitly and explain the better target.

---

# Stop Conditions

STOP and do not write ontology if:

- semantic initialization is incomplete
- the concept appears in only one issue or note
- the relationship model is speculative
- terminology conflicts with `GLOSSARY.md`
- invariant impact is unclear
- lifecycle authority would be weakened
- external/advisory systems would become canonical by implication
- existing topics or entities are sufficient
- contradictions exist and have not been surfaced

---

# Anti-Patterns

Avoid:

- ontology files for individual runtime classes
- ontology files that merely restate processed notes
- promoting every important term into ontology
- formalizing strategy before it stabilizes
- hiding uncertainty to make a clean model
- duplicating entity definitions
- using ontology to bypass ADRs
- changing terminology without glossary review
- treating ontology as globally active runtime context

---

# Final Principle

Ontology exists to preserve stable conceptual structure.

The goal is not more files.

The goal is to make durable relationships explicit when TradeForge has learned
enough to model them without distorting the system.
