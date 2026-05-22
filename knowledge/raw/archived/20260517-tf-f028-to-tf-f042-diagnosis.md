---
title: TF-F028 To TF-F042 Diagnosis - Opportunity Workspace Feedback Cluster
type: brainstorm
status: raw
tags: [tradeforge, diagnosis, feedback-cluster, opportunity-workspace, ux]
created: 2026-05-17
---

# TF-F028 To TF-F042 Diagnosis - Opportunity Workspace Feedback Cluster

## Source Feedback

- `knowledge/raw/brainstorm-20260517-opportunity-workspace-ux-and-context-acquisition.md`
- `knowledge/raw/archived/Brainstorm session 20260517 on the Opportunity Workspace.md`
- `knowledge/raw/brainstorm session Screenshot 2026-05-17 135450.png`
- `knowledge/raw/brainstorm session Screenshot 2026-05-17 142020.png`

## Observable Gap

The current Opportunity Workspace is structurally correct in several architectural respects, but the operator experience is still too close to runtime internals:

- internal ontology appears directly in trader-facing language
- instrument identity is weak
- provenance-oriented boxes dominate the main surface
- missing-context states are technically truthful but operationally unhelpful
- advisory context acquisition is fragmented and not yet represented as a clear workflow
- provider configuration and actual information retrieval are not clearly distinguished
- raw context data is displayed before interpretation
- the workspace is being asked to do both setup evaluation and research acquisition

## Diagnostic Split

The session does not describe one defect. It describes a feedback cluster spanning four bands:

1. **Local Opportunity Workspace UX**
   - instrument identity
   - misleading candidate language
   - provenance-first center surfaces
   - weak cognition guidance

2. **Context And Provider UX**
   - ambiguous unavailable states
   - lack of explicit context acquisition
   - missing provider-attempt transparency
   - lack of instrument-aware context types

3. **Interpretation And Synthesis**
   - raw provider payloads without operational interpretation
   - acquired context not feeding workspace synthesis

4. **Cross-System Doctrine / Architecture**
   - trader-language boundary
   - recovery-oriented missing-state doctrine
   - dedicated Context Workbench concept
   - context interpretation layer

## Relevant Invariants

- UX Is Architectural
- Workflow-Centric Architecture
- Workspaces Are Operational Environments
- Derived State Must Remain Distinguishable
- Terminology Stability
- Replayability Is Foundational

## Issue Decomposition

- `TF-F028` persistent instrument identity
- `TF-F029` candidate terminology
- `TF-F030` cognition-first Opportunity synthesis
- `TF-F031` unavailable-context interpretation
- `TF-F032` explicit context acquisition workflow
- `TF-F033` provider attempt transparency
- `TF-F034` equity versus ETF context distinction
- `TF-F035` scenario-language translation
- `TF-F036` discretionary-thinking guidance
- `TF-F037` context interpretation layer
- `TF-F038` Context Workbench concept
- `TF-F039` missing-information doctrine
- `TF-F040` trader-language doctrine
- `TF-F041` context-to-synthesis bridge
- `TF-F042` interpretation-first market-context presentation

## Why These Issues Stay Separate

- Copy issues should not silently absorb doctrine work.
- Local screen improvements should not become architecture changes by accident.
- Missing-state doctrine is broader than the specific fundamentals example.
- A future research workspace is a separate bounded context question, not a side-panel tweak.
- Interpretation-first market context is adjacent to, but not identical with, a full context interpretation architecture.

## Out Of Scope For This Triage Pass

- resolving any issue
- selecting milestone assignment
- implementing UI or service changes
- promoting doctrine into canonical docs before explicit design work
