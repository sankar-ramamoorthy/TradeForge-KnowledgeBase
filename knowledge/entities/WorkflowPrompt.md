---
title: Workflow Prompt
type: entity
status: draft
tags: [TradeForge, entity, prompt, workflow, semantic-governance]
created: 2026-05-09
updated: 2026-05-09
aliases: [Prompt, Workflow Prompts]
related:
  - "[[CodexSkill]]"
  - "[[Codex Operating Boundaries]]"
  - "[[Runtime KB Development Loop]]"
---

# Workflow Prompt

## Definition

A Workflow Prompt is a reusable operational invocation surface for Codex.

Prompts define what Codex should do in a repeatable task context, such as planning runtime work, processing KB notes, synchronizing operational state, or implementing a bounded issue.

## Semantic Role

Workflow Prompts operationalize playbooks. They translate governance doctrine into repeatable execution procedures for Codex sessions.

Prompts are not reusable reasoning capabilities. They may invoke skills, but they should not absorb skill responsibilities.

## Authority Boundary

A prompt may sequence work, require context loading, and specify outputs.

A prompt must not:

- redefine canonical ontology
- override invariants
- collapse lifecycle authority
- convert advisory AI behavior into canonical authority
- replace playbooks as the source of governance intent

## Workflow Relationship

Prompts define executable procedural flow.

The durable distinction is:

```text
skills  = reusable thinking constraints
prompts = reusable operational workflows
```

## Repository Boundary

Workflow prompts belong in:

```text
prompts/
```

Prompts are KB artifacts because they preserve reusable operational procedure for semantic stabilization and runtime alignment work.

## Replay And Review Relevance

Prompts support development replayability by making task entrypoints reproducible. A future replay should be able to reconstruct which workflow prompt governed a processing, planning, implementation, or synchronization pass.

## Related Concepts

- [[CodexSkill]]
- [[Codex Operating Boundaries]]
- [[Development Replayability]]
- [[Runtime KB Development Loop]]
