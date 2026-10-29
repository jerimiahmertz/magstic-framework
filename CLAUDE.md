# GSTIC Framework — Claude Code Instructions

---

## What This Is

This repo contains the **GSTIC Strategic Planning Framework** — a structured methodology for market discovery, strategic planning, and execution design. The framework consists of two major stages:

### Stage 1: Builder PM Stack (Phase 0)
Market discovery and validation — assess whether a market is worth entering, size the opportunity, understand the customer, define positioning, and model the business.

| Phase | Focus | Key Question |
|-------|-------|-------------|
| **0A** — Market Attractiveness | Macro forces, competitive dynamics, value chain | Should we be in this market? |
| **0B** — Opportunity Sizing | TAM / SAM / SOM | How big is the opportunity? |
| **0C** — Customer Discovery | Segmentation, JTBD, switching forces | Who exactly, and what do they need? |
| **0D** — Value & Positioning | Value prop, Blue Ocean, Playing to Win | What's our right to win? |
| **0E** — Business Model | Lean Canvas, unit economics, experiments | How does this become a business? |

### Stage 2: GSTIC Strategic Planning (Phases 1–5)
Define objectives, develop strategy, plan tactics, build execution roadmap, and design measurement systems.

| Phase | Focus | Key Question |
|-------|-------|-------------|
| **G** — Goals | Objectives & KPIs | What does success look like? |
| **S** — Strategy | Audience & positioning | Who are we targeting and why us? |
| **T** — Tactics | Marketing & product mix | What levers do we pull? |
| **I** — Implementation | Roadmap & execution | How and when do we deliver? |
| **C** — Control | Measurement & iteration | How do we know it's working? |

---

## How to Use This Framework

When a user asks to run the framework, or a specific phase, for a project:

1. **Read the kickoff prompt** at `prompts/kickoff.md` — this is your orchestration guide for every mode below
2. **Gather context once** (kickoff Step 0) before the first phase, whatever the entry point
3. **Activate agents** — each phase has a dedicated agent persona in `agents/`
4. **Use templates** from `templates/` to structure outputs at each phase
5. **Follow phase transitions** defined in `prompts/phase-transitions.md` — check each phase's exit criteria before handing off
6. **Save each deliverable** to `output/<project-slug>/` once the user confirms it

### Full Pipeline ("Run full pipeline for [project]")
1. Start with Phase 0A and proceed sequentially through 0E
2. Use the Market Discovery Brief Summary from 0E as the entry document for Phase 1
3. Continue through Phases 1–5 (Goals → Strategy → Tactics → Implementation → Control)

### GSTIC Only ("Run GSTIC for [project]")
1. Start at Phase 1: Goals and proceed through Phase 5: Control
2. If market context is thin, suggest running Phase 0 first

### Builder PM Stack Only ("Run market discovery for [project]")
1. Start at Phase 0A and proceed through 0E
2. Produce the Market Discovery Brief Summary as the final deliverable

### Single Phase ("Run Phase 0C for [project]" or "Run Tactics for [project]")
1. Jump to the requested phase
2. Warn which earlier phases are missing from `output/<project-slug>/`
3. State what assumptions you're making without upstream deliverables

---

## Agent Activation Rules

| Phase | Agent File | Role |
|-------|-----------|------|
| 0A–0E (Builder PM Stack) | `agents/market-analyst.md` | Market evaluation, sizing, customer discovery, positioning, business modeling |
| 1: Goals | `agents/strategist.md` | Defines objectives, KPIs, success criteria |
| 2: Strategy | `agents/strategist.md` | Identifies target segments, value prop, positioning |
| 3: Tactics | `agents/tactician.md` | Develops the marketing/product mix |
| 4: Implementation | `agents/tactician.md` | Builds roadmap, assigns resources, sets timelines |
| 5: Control | `agents/analyst.md` | Designs measurement framework and feedback loops |

---

## Template Mapping

### Phase 0: Builder PM Stack
| Phase | Template |
|-------|----------|
| 0A | `templates/market-attractiveness.md` |
| 0B | `templates/opportunity-sizing.md` |
| 0C | `templates/customer-discovery.md` |
| 0D | `templates/value-positioning.md` |
| 0E | `templates/business-model-canvas.md` |

### Phases 1–5: GSTIC
| Phase | Template |
|-------|----------|
| Goals | `templates/goals-worksheet.md` |
| Strategy | `templates/strategy-canvas.md` |
| Tactics | `templates/tactics-plan.md` |
| Implementation | `templates/implementation-roadmap.md` |
| Control | `templates/control-dashboard.md` |

### Compilation
| Deliverable | Template |
|-------------|----------|
| Full Brief | `templates/gstic-brief.md` |

---

## Orchestration Behavior

1. **Start at the mode's entry point by default.** Full pipeline and market discovery start at 0A. GSTIC-only starts at Goals.
2. **A named phase can run out of order.** If the user asks for a specific phase, run it — but first warn which earlier phases are missing and state the assumptions you're making in their place.
3. **Each phase must produce a deliverable** that meets its exit criteria in `prompts/phase-transitions.md` before transitioning to the next phase.
4. **Ask clarifying questions** at the start of each phase — but never re-ask what Step 0 or an earlier phase already answered.
5. **Reference prior phase outputs** when working on subsequent phases — read them from `output/<project-slug>/`.
6. **Offer iteration** at the end of each phase before moving on.
7. **Persona shift is explicit.** When changing agents, briefly acknowledge the shift in perspective.
8. If the user says "full pipeline" or "run all phases," proceed through all ten phases sequentially with brief confirmation between each.

---

## Saving Deliverables

| Phase | File |
|-------|------|
| 0A: Market Attractiveness | `output/<project-slug>/0a-market-attractiveness.md` |
| 0B: Opportunity Sizing | `output/<project-slug>/0b-opportunity-sizing.md` |
| 0C: Customer Discovery | `output/<project-slug>/0c-customer-discovery.md` |
| 0D: Value & Positioning | `output/<project-slug>/0d-value-positioning.md` |
| 0E: Business Model | `output/<project-slug>/0e-business-model.md` |
| 1: Goals | `output/<project-slug>/1-goals.md` |
| 2: Strategy | `output/<project-slug>/2-strategy.md` |
| 3: Tactics | `output/<project-slug>/3-tactics.md` |
| 4: Implementation | `output/<project-slug>/4-implementation.md` |
| 5: Control | `output/<project-slug>/5-control.md` |
| Compiled brief | `output/<project-slug>/gstic-brief.md` |

- `<project-slug>` is the project name in lowercase kebab-case (e.g., "Self-Serve Trial" → `self-serve-trial`).
- Save a phase only after the user confirms it. When a phase is revised, overwrite its file.
- The Market Discovery Brief Summary lives in Section 5 of `0e-business-model.md`.
- If `output/` holds more than one project, ask which one the user means.
- `output/` is git-ignored, so deliverables stay on the user's machine.

---

## Output Preferences

- Use Markdown for all deliverables
- Include tables for KPIs, timelines, and comparison matrices
- Keep language direct and actionable — no filler
- Tailor depth to the user's context (enterprise PM, startup, consulting, B2B, B2C)
- When applicable, reference integrated frameworks: Porter's Five Forces, Wardley Mapping, PESTEL, JTBD, Blue Ocean Strategy, Playing to Win, Hamilton Helmer's 7 Powers, Lean Canvas, Opportunity Solution Trees, HEART Framework, AARRR Pirate Metrics

---

## Project Context Detection

If the user mentions a specific project, try to understand:
- Is this B2B or B2C?
- Is this a new initiative or optimization of an existing one?
- What's the organizational scale? (startup, mid-market, enterprise)
- Are there existing metrics or baselines?
- Has any market discovery already been done?

Use these signals to calibrate depth, vocabulary, and which phases to emphasize.

---

## Quick-Access Commands

| Command | Action |
|---------|--------|
| `Run full pipeline for [project]` | Phase 0A through Phase 5 |
| `Run market discovery for [project]` | Phase 0A through 0E only |
| `Run GSTIC for [project]` | Phase 1 through 5 only |
| `Run [phase] for [project]` | Single phase — warns about missing earlier phases and states assumptions |
| `Revise [phase]` | Re-open a saved phase, update its file, and flag later phases that may need changes |
| `Show status` | List which phase files exist in `output/<project-slug>/` and name the next phase |
| `Compile brief` | Assemble the saved phases into `output/<project-slug>/gstic-brief.md` |
