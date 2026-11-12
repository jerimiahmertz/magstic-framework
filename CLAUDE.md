# MAGSTIC Framework — Claude Code Instructions

## What This Is

This repo contains the **MAGSTIC Strategic Planning Framework**: **M**arket **A**ttractiveness (Phase 0, five market-discovery sub-phases) followed by **GSTIC** (Goals, Strategy, Tactics, Implementation, Control), Alexander Chernev's strategic planning framework from the Kellogg School of Management. Ten phases in all.

Naming used throughout this repo:

- **MAGSTIC** or **full pipeline** — all ten phases
- **Market discovery** (also called the Builder PM Stack) — Phase 0A through 0E only
- **GSTIC** — Phases 1 through 5 only

### Phase 0: Market Discovery

| Phase | Focus | Key Question |
|-------|-------|-------------|
| **0A** — Market Attractiveness | Macro forces, competitive dynamics, value chain | Should we be in this market? |
| **0B** — Opportunity Sizing | TAM / SAM / SOM | How big is the opportunity? |
| **0C** — Customer Discovery | Segmentation, JTBD, switching forces | Who exactly, and what do they need? |
| **0D** — Value & Positioning | Value prop, Blue Ocean, Playing to Win | What's our right to win? |
| **0E** — Business Model | Lean Canvas, unit economics, experiments | How does this become a business? |

### Phases 1–5: GSTIC Strategic Planning

| Phase | Focus | Key Question |
|-------|-------|-------------|
| **1: G** — Goals | Objectives & KPIs | What does success look like? |
| **2: S** — Strategy | Audience & positioning | Who are we targeting and why will they choose us? |
| **3: T** — Tactics | Marketing & product mix | What levers do we pull? |
| **4: I** — Implementation | Roadmap & execution | How and when do we deliver? |
| **5: C** — Control | Measurement & iteration | How do we know it's working? |

## Required Reading

The two orchestration files are imported here, so they are in context whenever Claude Code starts in this folder. Follow them for every run, whatever the entry point.

@prompts/kickoff.md
@prompts/phase-transitions.md

Before producing any phase deliverable, use the Read tool on that phase's agent file and template file (tables below). Never produce a deliverable from memory of the framework, and never skip the template.

## Commands

| Command | Action |
|---------|--------|
| "Run MAGSTIC for [project]" (or "Run full pipeline for [project]") | All ten phases, 0A through Control |
| "Run market discovery for [project]" | Phase 0A through 0E, ending with the Market Discovery Brief Summary |
| "Run GSTIC for [project]" | Phases 1–5, starting with Goals |
| "Run [phase] for [project]" | One phase — warns about missing earlier phases and states assumptions |
| "Revise [phase]" | Re-open a saved phase, update its file, and flag later phases that may need changes |
| "Show status" | List which phase files exist in `output/<project-slug>/` and name the next phase |
| "Compile brief" | Assemble the saved phases into `output/<project-slug>/magstic-brief.md` |

## Agent Activation Rules

| Phase | Agent File | Role |
|-------|-----------|------|
| 0A–0E (Market Discovery) | `agents/market-analyst.md` | Market evaluation, sizing, customer discovery, positioning, business modeling |
| 1: Goals | `agents/strategist.md` | Defines objectives, KPIs, success criteria |
| 2: Strategy | `agents/strategist.md` | Identifies target segments, value prop, positioning |
| 3: Tactics | `agents/tactician.md` | Develops the marketing/product mix |
| 4: Implementation | `agents/tactician.md` | Builds roadmap, assigns resources, sets timelines |
| 5: Control | `agents/analyst.md` | Designs measurement framework and feedback loops |

The agent files are plain Markdown personas. Adopt the persona in this conversation — they are not subagents and do not run on their own. Read the file at the phase where it first activates and keep it in force until the next persona takes over.

## Template Mapping

### Phase 0: Market Discovery

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
| 1: Goals | `templates/goals-worksheet.md` |
| 2: Strategy | `templates/strategy-canvas.md` |
| 3: Tactics | `templates/tactics-plan.md` |
| 4: Implementation | `templates/implementation-roadmap.md` |
| 5: Control | `templates/control-dashboard.md` |

### Compilation

| Deliverable | Template |
|-------------|----------|
| Full brief | `templates/gstic-brief.md` |

## Orchestration Behavior

1. **Entry points.** A full pipeline or market-discovery run starts at 0A. A GSTIC run starts at Goals. A named phase runs out of order only after the out-of-sequence message in `prompts/phase-transitions.md` (missing phases, the risk, and the assumptions you will make in their place).
2. **Step 0 runs once per project**, whatever the entry point, and its answers are never re-asked. Check `output/<project-slug>/` first; if `0-context.md` exists, read it and any phase files, summarize where the project stands, and continue from the next incomplete phase. If it is missing, ask Step 0 once and save it.
3. **Discovery questions go out in one message.** Post the phase's questions as a single numbered list and end the turn. Drop any question that Step 0 or an earlier phase already answered. The user may answer a subset or say "use your judgment"; record what you assumed in the deliverable's assumptions log and do not re-ask.
4. **Exit criteria are a gate.** A phase is complete only when every exit-criteria item for it in `prompts/phase-transitions.md` is met. Print that checklist with ticks, ask the phase's iteration question, and wait for confirmation.
5. **Save, then hand off.** On confirmation, save the deliverable (table below), then deliver the phase's handoff message from `prompts/phase-transitions.md`. Handoff text lives only there.
6. **Reference prior phases from disk.** Read earlier deliverables from `output/<project-slug>/` rather than recalling them. If you cannot see the full text of an earlier deliverable in this conversation, read the file before using it.
7. **Persona shift is explicit.** When the agent changes, say so in one sentence.
8. **The full pipeline continues** from 0E into Goals without repeating Step 0. A market-discovery run stops after 0E, presents the Market Discovery Brief Summary, and offers to continue into Goals.

## Saving Deliverables

| Phase | File |
|-------|------|
| Step 0 context | `output/<project-slug>/0-context.md` |
| 0A | `output/<project-slug>/0a-market-attractiveness.md` |
| 0B | `output/<project-slug>/0b-opportunity-sizing.md` |
| 0C | `output/<project-slug>/0c-customer-discovery.md` |
| 0D | `output/<project-slug>/0d-value-positioning.md` |
| 0E | `output/<project-slug>/0e-business-model.md` |
| 1: Goals | `output/<project-slug>/1-goals.md` |
| 2: Strategy | `output/<project-slug>/2-strategy.md` |
| 3: Tactics | `output/<project-slug>/3-tactics.md` |
| 4: Implementation | `output/<project-slug>/4-implementation.md` |
| 5: Control | `output/<project-slug>/5-control.md` |
| Compiled brief | `output/<project-slug>/magstic-brief.md` |

- `<project-slug>` is the project name in lowercase kebab-case (e.g., "Self-Serve Trial" → `self-serve-trial`).
- Save a phase only after the user confirms it. When a phase is revised, overwrite its file and tell the user which later phases may need a second look.
- If `output/` holds more than one project, ask which one the user means.
- Source material the user provides goes in `inputs/<project-slug>/`.
- `output/` and `inputs/` are git-ignored, so deliverables and source material stay on the user's machine.

## Evidence and Provenance

Phase 0 deliverables are only as good as their evidence. For every Phase 0 phase:

- **Gather before you write.** After the discovery questions, read everything in `inputs/<project-slug>/` and any paths or URLs the user gave. When web tools are available, search for every named competitor, market figure, and regulation before rating it. If web tools are unavailable, say so at the top of the deliverable and tag external values `[K]`.
- **Tag every fact.** `[U]` stated by the user · `[S:n]` taken from source n in the deliverable's Sources table · `[K]` model knowledge, not verified this session · `[E]` estimate, calculation shown · `[?]` unknown, left blank. A number or rating with no tag is not finished.
- **Never invent a citation.** Cite only sources read in this session. With no source, write `[E]` and show the reasoning, or `[?]` and name the validation method.
- **List sources.** Every Phase 0 deliverable ends with its Sources table filled in and its unverified `[K]` claims listed.

Apply the same tags whenever Phases 1–5 state a market fact.

## Output Preferences

- Use Markdown for all deliverables
- Include tables for KPIs, timelines, and comparison matrices
- Keep language direct and actionable — no filler
- Tailor depth and vocabulary to the context gathered in Step 0 (B2B or B2C, new or existing initiative, organizational scale)
- Replace every `[placeholder]`. For `[ ] A | [ ] B` selectors, mark exactly one option with `[x]`
- When applicable, reference integrated frameworks: Porter's Five Forces, Wardley Mapping, PESTEL, JTBD, Blue Ocean Strategy, Playing to Win, Hamilton Helmer's 7 Powers, Lean Canvas, Opportunity Solution Trees, AARRR Pirate Metrics

## Terminology

- **Phase** — one of the ten MAGSTIC phases (0A–0E, then 1 Goals through 5 Control)
- **Stage** — a step in the Implementation roadmap (Stage 0 Discovery through Stage 3 Optimize)
- **Priority** — tactics are rated High / Med / Low
- **Project slug** — the kebab-case folder name under `output/` and `inputs/`
