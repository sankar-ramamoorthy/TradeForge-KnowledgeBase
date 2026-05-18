---
title: Context Workbench
status: canonical
type: design-architecture
created: 2026-05-17
updated: 2026-05-17
tags:
  - TradeForge
  - design
  - workspace
  - context-workbench
  - research
related:
  - ADR-0040
  - WORKSPACES.md
  - context-interpretation-layer.md
  - missing-information-doctrine.md
authoritative_statement: >
  The Context Workbench is the research-oriented operational workspace for
  acquiring, inspecting, interpreting, and preparing advisory context before
  later decision workflow stages.
---

# Context Workbench

## Operational Question

> What do I need to know about this instrument before deciding how to interpret it?

## Purpose

The Context Workbench is a research-oriented operational environment. It exists
to gather and organize advisory context before that context is selectively used
inside opportunity evaluation or thesis formation.

## Owns

- context acquisition requests
- context-family visibility
- provider-attempt status
- retrieval outcomes
- interpretation of fetched advisory context
- relevance framing for later workspaces

## Does Not Own

- lifecycle authority
- thesis authoring
- trade planning
- execution decisions
- canonical event truth

## Candidate Context Families

- price / technical context
- fundamentals
- company profile
- ETF-relevant context
- sector / peer context
- catalysts and earnings context
- future news or macro context

## Workspace Relationship

- `Context Workbench`: acquire and understand context
- `Opportunity Workspace`: evaluate whether a setup is worth developing
- `Thesis` flow: structure the operator's reasoning

## Required Surface Qualities

- persistent instrument identity
- explicit context-family states
- provider attempts and fallback outcomes
- interpretation-first presentation with access to raw detail
- ability to mark context as relevant for later workflow use

## Non-Goals

- dashboard-style data sprawl
- automatic conviction generation
- replacing opportunity evaluation
- hiding uncertainty or missing context

