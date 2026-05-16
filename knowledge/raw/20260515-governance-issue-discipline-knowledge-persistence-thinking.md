# Raw Thinking: Issue Discipline Rule + Knowledge Persistence Strategy

**Date:** 2026-05-15
**Status:** Raw — needs further thinking before canonicalization
**Trigger:** Last-session conversation; rule currently lives only in personal memory files

---

## The Problem

Behavioral rules and governance decisions exist only in Claude's personal memory files.

Personal memory files are:
- invisible to Codex
- invisible to fresh Claude sessions
- invisible to any other AI tool
- not part of the KB semantic system

This makes rules fragile and single-tool.

---

## The Rule (Canonical Text)

The user stated this formally:

> No behavioral, architectural, workflow, or runtime code change may occur without a corresponding issue.
>
> GitHub Issues are preferred. DOCS/ISSUE_REGISTER.md works well for offline or exploratory work.
>
> All issues must classify the change as:
> - bug
> - enhancement
> - architectural
> - doctrine
> - refactor
> - operational
>
> Implementation must remain traceable to declared intent.

This rule applies to Claude, Codex, and any other AI agent working in this or the runtime repository. No exceptions.

---

## The Broader Problem: Multi-Tool Persistence

Rules need to survive across:
- AI tool changes (Claude → Codex → future tools)
- Fresh sessions (cold start, no memory)
- Different repos (KB repo vs runtime repo)

The user's stated intent: "we need other mechanisms and also need to use the KB."

---

## Thinking: Tier Model for Knowledge Persistence

A tiered model where each tier serves a different consumer:

| Tier | Mechanism | Who Reads It | When |
|---|---|---|---|
| 1 | `CLAUDE.md` / `AGENTS.md` in each repo | Any AI tool | Boot time, always |
| 2 | KB root-level doctrine docs | KB readers, doctrine consumers | On relevant task load |
| 3 | ADRs in runtime repo | Architectural decision traceability | Decision-level context |
| 4 | GitHub Issues / `DOCS/ISSUE_REGISTER.md` | Operational tracking | Per-change |
| 5 | Personal memory files | Session-specific ephemera only | Current session only |

Personal memory files must not store rules. Rules need to be at Tier 1 or 2 to survive tool changes.

---

## Thinking: What Goes Where

### CLAUDE.md / AGENTS.md (Tier 1)

**Constraint:** User wants to keep these small. Progressive discovery preferred.

Options:
- Add a minimal Issue Discipline section (5-10 lines) that references the full doctrine doc
- Use CLAUDE.md as a pointer, not a content holder
- The full rule text lives in Tier 2 (KB doctrine doc)

The boot-time file should say: "there is an issue discipline rule, see DEVELOPMENT_GOVERNANCE.md."

### KB Doctrine Document (Tier 2)

A new root-level file alongside INVARIANTS.md and SEMANTIC_GOVERNANCE.md:

**Candidate name:** `DEVELOPMENT_GOVERNANCE.md`

This is parallel to:
- SEMANTIC_GOVERNANCE.md (governs KB maturation)
- But governs development process discipline

Content would include:
- Issue-first rule (full canonical text)
- Issue classification taxonomy with examples
- The knowledge persistence tier model
- What goes in memory files vs KB vs ADRs vs CLAUDE.md
- Anti-patterns: silent fixes, retroactive issues, rules in memory only

**Open question:** Does this belong in root-level KB or in `knowledge/index/`?

Root-level feels right — it's parallel to INVARIANTS.md and SEMANTIC_GOVERNANCE.md.

### runtime-context-map.md update

A new context bundle entry for "Development Governance Work" to route tasks there:

```text
## Development Governance Work
Minimal required context:
- DEVELOPMENT_GOVERNANCE.md
- CLAUDE.md (Issue Discipline section)
- knowledge/index/ISSUE_REGISTER.md
```

### Runtime Repo (TradeForge) — separate session

Mirror the Issue Discipline pointer in `C:\Users\bosto\dockerstuff\TradeForge\CLAUDE.md`.

Also consider ADR-0024: Issue-First Development Discipline (sits at Tier 3 in authority hierarchy).

---

## Open Questions Before Canonicalization

1. **CLAUDE.md size constraint:** How minimal should the pointer be? Just a section header + one line referencing DEVELOPMENT_GOVERNANCE.md? Or a 5-line summary?

2. **Root vs index placement:** Does DEVELOPMENT_GOVERNANCE.md belong at KB root alongside INVARIANTS.md, or inside `knowledge/index/`?

3. **AGENTS.md:** Does a separate AGENTS.md need to exist (Codex convention) or is CLAUDE.md sufficient for both tools?

4. **Progressive discovery:** Should DEVELOPMENT_GOVERNANCE.md be loaded by default (added to the Mandatory Semantic Boot Sequence) or only loaded when task category is "development governance"?

5. **Runtime repo ADR:** Is ADR-0024 the right home for this decision, or is this better as a governance doc than an ADR?

6. **Retrospective issue:** The 422 fix made last session needs a retrospective issue before its commit goes in. That's a concrete action item before implementation resumes.

---

## Tentative Recommendation (to revisit)

1. Add a minimal 5-line Issue Discipline section to CLAUDE.md — just the hard rule + "See DEVELOPMENT_GOVERNANCE.md"
2. Create DEVELOPMENT_GOVERNANCE.md at KB root with full doctrine
3. Update runtime-context-map.md with Development Governance context bundle
4. Separate session: mirror in runtime repo CLAUDE.md
5. Separate session: consider ADR-0024

Not ready to canonicalize yet. User wants to think through the CLAUDE.md size constraint and progressive discovery approach first.

---

## Status

Raw capture. Not canonical.

Do not promote until:
- User confirms preferred approach for CLAUDE.md size/scope
- DEVELOPMENT_GOVERNANCE.md placement confirmed
- AGENTS.md question resolved
- Progressive discovery vs always-load decision made
