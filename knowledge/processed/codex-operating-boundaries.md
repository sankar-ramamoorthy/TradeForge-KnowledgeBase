---
title: Codex Operating Boundaries
type: note
status: processed
tags: [TradeForge, Codex, skills, prompts, operating-boundaries]
created: 2026-05-08
updated: 2026-05-09
related:
  - "[[CodexSkill]]"
  - "[[WorkflowPrompt]]"
  - "[[Runtime KB Development Loop]]"
  - "[[Development Replayability]]"
  - "[[INVARIANTS]]"
---

# Codex Operating Boundaries

## Purpose

This note preserves operating guidance for where Codex should work and how to distinguish skills from prompts in the TradeForge knowledge system.

---

## Repository Boundary Rule

Use the knowledge base when modifying semantic and cognitive structure:

- semantic doctrine
- prompts
- ontology
- skills
- workflows
- cognition structure
- durable architectural memory

Use the runtime repository when modifying implementation structure:

- runtime ADRs
- services
- domain models
- runtime documentation
- source code
- implementation tests

The practical split is:

- `knowledge-base/TradeForge/` preserves why concepts exist and what they mean.
- `TradeForge/` implements how the system behaves and when work is sequenced.

---

## Skills Versus Prompts

Skills and prompts should remain separate because they operate at different layers.

Skills define how Codex should think:

- behavioral modifiers
- reasoning constraints
- cognitive overlays
- domain operating rules
- architectural discipline

Examples:

- decision lifecycle reasoning
- event-sourcing discipline
- execution discipline
- market interpretation
- scenario ranking
- workspace querying

Skills belong in:

```text
skills/
```

Prompts define what Codex should do:

- reusable workflows
- operational procedures
- structured task entrypoints

Examples:

- generate ADRs
- build a vertical slice
- replay analysis

Prompts belong in:

```text
prompts/
```

---

## Design Rule

Do not merge skills and prompts.

The durable distinction is:

```text
skills  = reusable thinking constraints
prompts = reusable operational workflows
```

This separation protects terminology discipline and keeps behavioral guidance distinct from task execution.

---

## KB Processing Result

This note introduces a stable operating boundary between two KB artifact types:

- [[CodexSkill]] for reusable reasoning constraints
- [[WorkflowPrompt]] for reusable operational workflows

The distinction is ontology-bearing because it defines different semantic responsibilities inside the TradeForge knowledge system.

---

## Ontology Implications

Skills and prompts should be modeled as separate KB artifact types, not interchangeable documentation labels.

Ontology implications:

- [[CodexSkill]] belongs to reusable cognition and reasoning discipline.
- [[WorkflowPrompt]] belongs to repeatable operational procedure.
- playbooks define governance intent and doctrine.
- prompts operationalize playbooks for repeatable Codex execution.
- skills may be invoked by prompts but should not become task procedures.

This prevents ontology drift where agent behavior rules, workflow procedures, and governance doctrine collapse into one artifact type.

---

## Workflow Implications

Reusable Codex work should follow a clear sequence:

```text
playbook doctrine
    -> workflow prompt
    -> skill activation
    -> task execution
    -> KB processing or runtime synchronization
```

Workflow implications:

- `prompts/workflow/` should remain the home for procedural Codex entrypoints.
- `skills/` should remain the home for reusable reasoning constraints.
- prompts may require skills as part of execution, but skills should not contain full workflow orchestration.
- KB processing should check whether new guidance belongs in a playbook, workflow prompt, skill, topic, or entity before promotion.

---

## Playbook Implications

The [[Runtime KB Development Loop]] should preserve this artifact boundary because it governs how runtime work and KB stabilization remain replayable.

Playbook-level rules implied by this note:

- do not merge skill behavior into workflow prompts unless it is only invocation guidance.
- do not place procedural task sequences in skills when they belong in prompts.
- do not treat prompts or skills as canonical doctrine when a playbook, invariant, ADR, or entity definition is the correct authority.
- preserve source and activation context so future development replay can reconstruct which reasoning constraints and operational prompts governed a task.

---

## Doctrine Integration Opportunities

This note reinforces existing doctrine rather than changing it.

Relevant doctrine links:

- [[INVARIANTS]]: terminology stability, AI advisory boundary, replayability, and workflow-centric architecture.
- [[ARCHITECTURE]]: progressive discovery knowledge model and semantic linking.
- [[GLOSSARY]]: terminology governance rule.
- [[EVENT_TAXONOMY]]: prompts and skills are not events; they are governance and cognition artifacts.
- [[Development Replayability]]: development work must be reconstructable from prompts, skills, playbooks, processed notes, ADRs, and commits.

No contradiction with canonical doctrine was identified.

---

## Cross-Linking Opportunities

Stable cross-links created or recommended:

- [[CodexSkill]]
- [[WorkflowPrompt]]
- [[Runtime KB Development Loop]]
- [[Development Replayability]]
- [[KB Processing Workflow Prompt]]

Future index work should include a dedicated workflow and artifact map once more prompts and skills are stabilized.
