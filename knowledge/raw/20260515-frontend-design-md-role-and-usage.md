# Frontend DESIGN.md — Role and Usage

**Date:** 2026-05-15
**Status:** Raw — captured for future reference (hiring managers, developers, onboarding)

---

## What Is frontend/DESIGN.md?

`TradeForge/frontend/DESIGN.md` is the frontend design system reference document.

It was inspired by Google's DESIGN.md pattern but scoped specifically to frontend concerns.
It is explicitly NOT semantic doctrine — the knowledge base and ADRs remain authoritative
for workspace meaning, lifecycle authority, replay, and event truth.

Its job is to give any developer or AI agent building a frontend page the answers to:

- What colors to use
- What spacing to use
- What typography to apply
- What layout constraints to follow
- What visual principles to respect

---

## What It Contains

### Design Tokens (YAML frontmatter)

```yaml
tokens:
  color:
    canvas: "#eef1ef"        # page background
    surface: "#ffffff"       # card/panel background
    surface_muted: "#f9fbfa"
    surface_warm: "#f6f2eb"
    text: "#18201f"
    text_muted: "#56625f"
    text_subtle: "#6f7976"
    border: "#d5dcd8"
    border_active: "#2f7669"
    accent: "#2f7669"        # primary brand green
    accent_surface: "#edf7f2"
    attention: "#856432"     # amber / warning
    attention_surface: "#f4efe6"
    danger: "#b45f3a"        # error / risk
    danger_surface: "#fbede6"
  radius:
    control: "8px"
    surface: "8px"
  spacing:
    page_inline: "32px"
    panel: "24px"
    stack: "16px"
    compact: "10px"
  typography:
    family: "Inter, ui-sans-serif, system-ui, ..."
    letter_spacing: "0"
    body_line_height: "1.6"
layout:
  shell_max_width: "1220px"
  sidebar_width: "280px"
  breakpoint_stack: "860px"
  breakpoint_compact: "560px"
```

### Visual Principles (prose)

- Context before action
- Dense but readable operational surfaces
- Routes and panels should preserve decision continuity
- Cards are for individual operational surfaces only
- Avoid dashboard-style metric grids and detached action controls
- Keep radius at 8px or less
- Letter spacing remains 0

---

## How It Is Actually Used — The Token Chain

The tokens flow through the system in three steps:

```
frontend/DESIGN.md   (token definitions — source of truth)
        ↓
frontend/src/styles.css   (CSS custom properties — mirrors token names)
        ↓
React components   (reference class names — inherit token values)
```

### Step 1: DESIGN.md defines the tokens

The YAML frontmatter in `frontend/DESIGN.md` defines every color, spacing, radius, and
typography value by name.

### Step 2: styles.css mirrors them as CSS custom properties

```css
:root {
  --color-canvas: #eef1ef;
  --color-accent: #2f7669;
  --space-panel: 24px;
  --radius-surface: 8px;
  --layout-sidebar: 280px;
  ...
}
```

The class names then use the variables:

```css
.workspace-briefing {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-surface);
  padding: var(--space-panel);
}
```

### Step 3: React components use class names

```tsx
<section className="workspace-briefing">
<main className="app-shell">
<div className="authority-strip">
```

Components never reference hex values or token values directly.
They use semantic class names that inherit the token values through CSS.

---

## Is It Working?

Yes. The token system is live and consistent across the frontend.

No component uses hardcoded hex colors — they all go through class names, which go
through CSS custom properties, which are defined from the DESIGN.md token set.

**One known inconsistency found (2026-05-15):**

```css
.eyebrow {
  color: #4f6f67;  /* hardcoded — should be var(--color-text-muted) */
}
```

The `.eyebrow` class has a hardcoded color instead of using `var(--color-text-muted)`.
This is a minor drift from the system. Low priority but worth cleaning up eventually.

---

## Authority Boundary

`frontend/DESIGN.md` defines:
- frontend design tokens
- reusable layout primitives
- component composition rules
- visual hierarchy for operational surfaces
- constraints that prevent dashboard drift

`frontend/DESIGN.md` does NOT define:
- workspace ontology (→ KB WORKSPACES.md)
- lifecycle semantics (→ KB INVARIANTS.md + ADRs)
- event meaning (→ KB EVENT_TAXONOMY.md)
- persona behavior (→ KB PERSONAS.md)
- replay authority (→ KB doctrine + ADRs)
- product philosophy beyond runtime translation (→ KB DESIGN_ARCHITECTURE.md)

---

## Relationship to KB Design Architecture

There are two design layers in TradeForge:

| Layer | File | Purpose |
|---|---|---|
| Design doctrine | `knowledge-base/TradeForge/design/DESIGN_ARCHITECTURE.md` | Cognitive interaction architecture, workspace composition, operational surfaces — the WHY |
| Frontend translation | `TradeForge/frontend/DESIGN.md` | Tokens, spacing, layout rules — the HOW for the frontend |

The KB document defines what TradeForge's design means.
The frontend document translates that meaning into implementation-ready values.

---

## Why This Approach

Defining tokens in one place (`DESIGN.md`) and flowing them through CSS custom properties
means:

- changing a color requires editing exactly one value in one place
- any developer or AI agent building a new page knows exactly what to use
- the visual system stays consistent without requiring framework-level tooling (no Tailwind, no CSS-in-JS)
- the separation between "what the system means" (KB doctrine) and "how it looks" (frontend tokens) is explicit

---

## For Hiring Managers / Developers

**Q: Do you have a design system?**

Yes. Design tokens are defined in `frontend/DESIGN.md` and flow through CSS custom
properties in `styles.css` to all React components via semantic class names.

**Q: How do you keep the frontend consistent?**

Every color, spacing, and layout value is defined once as a named token. Components use
class names, not hardcoded values. Changing the accent color means editing one line.

**Q: How does the frontend relate to the broader architecture?**

The frontend is the visible projection of workspace state. Design decisions that affect
what TradeForge means (workspace boundaries, lifecycle authority, persona behavior) live
in the knowledge base and ADRs — not in frontend code. The frontend translates doctrine
into UI. It does not own doctrine.

---

## Relationship To Workspace Operational Layouts

A third design layer exists between cognitive doctrine and frontend implementation:

| Layer | File | Responsibility |
|---|---|---|
| Cognitive doctrine | `knowledge-base/TradeForge/design/DESIGN_ARCHITECTURE.md` | Defines what TradeForge fundamentally is |
| Operational workspace architecture | `knowledge-base/TradeForge/design/workspace-operational-layouts.md` | Defines how operational cognition becomes workspace composition |
| Frontend implementation translation | `TradeForge/frontend/DESIGN.md` | Defines tokens, spacing, visual primitives, and implementation constraints |

This separation is intentional.

TradeForge treats:

```text
meaning
interaction architecture
visual implementation
```

as distinct architectural concerns.

The frontend should never become the canonical owner of:

- workspace meaning
- lifecycle semantics
- operational authority
- cognitive doctrine

Instead, frontend implementation translates operational architecture into visible UI structures.
