---
title: Research Cockpit Repository Boundary
type: brainstorm
status: raw
tags: [tradeforge, brainstorm, research-cockpit, repository-boundary, advisory-artifacts]
created: 2026-07-13
---

# Research Cockpit Repository Boundary

## Trigger

Several ChatGPT brainstorming sessions developed a vision for a Research
Cockpit upstream of TradeForge. The notes are currently staged outside the
TradeForge workspace while deciding whether the cockpit should remain a
knowledge-base concept, become part of the runtime, or become a third sibling
repository.

Recent TradeForge work changes the decision context. Advisory observations,
candidates, evidence, research imports, watchlists, evidence panels, and
transparent attention ranking now exist in the runtime. The remaining question
is where the system that discovers candidates and produces deeper research
artifacts should live.

## Raw Input

The operator's current position is:

> The advice has been to create brainstorming notes in the raw folder of the
> knowledge-base repository before creating a third sibling for research. I
> feel that a third repository is necessary, but I have kept the current
> research workspace outside `TradeForge-Project` until that boundary is
> reviewed.

The research vision developed across the staged notes includes:

- an upstream research feeder for TradeForge;
- machine-assisted candidate discovery and attention expansion;
- source-backed observations, claims, evidence, risks, and catalysts;
- thesis packets or other advisory artifacts for explicit operator review;
- possible ontology, normalization, append-only research history, and later
  graph materialization;
- no direct ownership of TradeForge lifecycle state or execution authority.

The proposed workspace shape is:

```text
TradeForge-Project/
  TradeForge/
  TradeForge-KnowledgeBase/
  TradeForge-ResearchCockpit/
```

Each child would remain an independent Git repository.

## Observations

- The knowledge base is Markdown-only semantic authority. Executable ingestion,
  extraction, normalization, provider, scheduling, and artifact-generation code
  does not belong there.
- The TradeForge runtime is the governed decision-lifecycle consumer. Putting
  experimental research machinery inside it would blur the boundary between
  advisory discovery and operator commitment.
- M12 and M14C already provide non-canonical advisory artifact storage and
  operator-controlled thesis and plan import workflows.
- EV-00 through EV-05 provide Evidence Density for symbols already referenced
  by active decisions or operator watchlists. They do not perform broad market
  discovery or long-form, claim-oriented research.
- The July 9 product audit distinguishes the advanced Codex/cockpit operator
  from family users who need simpler in-app capture and onboarding.
- A third repository has a separate data lifecycle, experimental cadence,
  dependency profile, and failure boundary. This is a stronger reason for
  separation than project size alone.
- The current staged research directory should not become the implementation
  repository merely because it contains the source brainstorm transcripts.

## Ideas

- Create `TradeForge-ResearchCockpit` as a third independent sibling after the
  repository boundary is reviewed and accepted.
- Keep cross-system meaning in `TradeForge-KnowledgeBase`: authority rules,
  ontology, terminology, artifact semantics, and architectural decisions.
- Let `TradeForge-ResearchCockpit` own research execution: source ingestion,
  screening, extraction, normalization, provenance, local research state, and
  advisory artifact generation.
- Let the TradeForge runtime own the accepted import contract, validation,
  operator review, lifecycle promotion, and canonical events.
- Use versioned artifact schemas and contract fixtures rather than importing
  implementation code between repositories.
- Keep personal documents, provider caches, databases, and generated outputs
  out of Git. Commit only sanitized fixtures and example artifacts.
- Begin with one TradeForge-specific vertical slice. Do not begin with a generic
  research platform, graph database, multi-agent framework, or direct runtime
  database integration.
- Treat a graph database as a rebuildable query view only after the ontology and
  useful research questions have been proven with files and structured data.

The provisional dependency direction is:

```text
TradeForge-KnowledgeBase defines meaning
    -> TradeForge-ResearchCockpit produces advisory artifacts
    -> TradeForge validates, reviews, and imports them
```

## Questions

- What should the first cockpit vertical slice solve?
  - discovering symbols that the operator would otherwise miss;
  - assembling trustworthy evidence for a selected symbol or theme;
  - detecting changes that affect an existing thesis or position.
- Should the first output be an `AdvisoryCandidate`, a thesis packet, a market
  regime brief, or another existing M14C-compatible artifact?
- Which repository owns the machine-readable artifact schema file, and how will
  producer-consumer compatibility be tested across repositories?
- Should cockpit output remain in its own outbox until an explicit operator
  export, or may a command copy an artifact into TradeForge's import dropoff?
- Which data providers and source types are available, licensed, and useful
  enough for the first experiment?
- What evidence would demonstrate that the cockpit improves attention or
  decision quality rather than increasing information volume?
- Should the existing staged brainstorm files be preserved individually in raw
  knowledge, consolidated into a source note, or archived outside the active KB
  after synthesis?

## Concerns

- Three repositories increase coordination, documentation drift, and contract
  versioning costs.
- A cockpit can become a hidden recommendation engine if rankings or generated
  theses are presented with false authority.
- Continuous scanning and deep document research are distinct product paths;
  combining them in the first implementation could produce an unfocused system.
- Automated writes into TradeForge would weaken the advisory/commitment boundary
  even if the written objects are initially labeled non-canonical.
- Ontology and normalization infrastructure may be overbuilt before the system
  proves that its outputs answer useful operator questions.
- The cockpit could duplicate Evidence Density surfaces already implemented in
  TradeForge instead of adding upstream discovery or deeper research value.
- External research must preserve source identity, timestamps, provenance,
  uncertainty, caveats, and enough immutable input material for later replay.

## Possible Next Outputs

- Processed synthesis comparing repository placement alternatives.
- ADR candidate defining the three-repository boundary and authority ownership.
- Canonical topic update for [[Machine-Assisted Discretionary Cognition]].
- Import-contract ownership and compatibility-testing design note.
- Narrow MVP issue for the selected Research Cockpit vertical slice.
- Third-repository charter and local `AGENTS.md` after the boundary is approved.
