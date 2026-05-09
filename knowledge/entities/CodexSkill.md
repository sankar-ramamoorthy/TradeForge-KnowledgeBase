---
title: Codex Skill
type: entity
status: draft
tags: [TradeForge, entity, Codex, skills, semantic-governance]
created: 2026-05-09
updated: 2026-05-09
aliases: [Skill, Skills]
related:
  - "[[WorkflowPrompt]]"
  - "[[Codex Operating Boundaries]]"
  - "[[Runtime KB Development Loop]]"
---

# Codex Skill

## Definition

A Codex Skill is a reusable reasoning capability that constrains how Codex should think while operating inside TradeForge.

Skills encode behavioral modifiers, reasoning constraints, domain discipline, and architectural guardrails.

## Semantic Role

Skills preserve reusable cognition. They help future Codex sessions apply consistent interpretation across ontology, workflow, event-sourcing, replayability, UX, and AI-governance work.

Skills are not task entrypoints. They do not define a specific job to perform.

## Authority Boundary

A skill may guide reasoning, but it is not canonical doctrine by itself.

Skills must remain subordinate to:

- [[INVARIANTS]]
- [[GLOSSARY]]
- [[ARCHITECTURE]]
- [[EVENT_TAXONOMY]]
- accepted ADRs

Skills must not silently redefine ontology, lifecycle semantics, event authority, or AI governance boundaries.

## Workflow Relationship

Skills are activated by prompts, explicit user requests, or task context.

The durable distinction is:

```text
skills  = reusable thinking constraints
prompts = reusable operational workflows
```

## Repository Boundary

Skills belong in:

```text
skills/
```

They are KB artifacts because they preserve how agents should reason about TradeForge concepts.

Runtime implementation behavior belongs in the runtime repository, not in skills.

## Replay And Review Relevance

Skills improve development replayability by making agent reasoning constraints explicit and reusable. A future replay of development work should be able to identify which skills shaped reasoning and why.

## Related Concepts

- [[WorkflowPrompt]]
- [[Codex Operating Boundaries]]
- [[Development Replayability]]
- [[Runtime KB Development Loop]]
