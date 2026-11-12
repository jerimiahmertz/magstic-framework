# MAGSTIC Strategic Planning Framework

A structured, AI-driven strategic planning framework designed to run interactively via [Claude Code](https://code.claude.com/docs). MAGSTIC provides a complete pipeline from market discovery through execution measurement — ten phases in two parts.

**MAGSTIC** = **M**arket **A**ttractiveness + **GSTIC**. Phase 0 (five market-discovery sub-phases, also called the Builder PM Stack) validates the market. GSTIC (Goals, Strategy, Tactics, Implementation, Control), developed by Alexander Chernev at the Kellogg School of Management, then builds the strategic and operational plan. In this repo, "GSTIC" refers to Phases 1–5 only.

> **Just need GSTIC?** [gstic-framework](https://github.com/jerimiahmertz/gstic-framework) is the lighter, five-phase version without Phase 0.

## What is MAGSTIC?

### Phase 0: Market Discovery

Before you build strategy, validate the market. Phase 0 uses five sub-phases to determine whether an opportunity is worth pursuing and how to position within it.

| Phase | Focus | Frameworks Used | Key Question |
|-------|-------|----------------|-------------|
| **0A** — Market Attractiveness | Macro forces, competitive dynamics, value chain | PESTEL, Porter's Five Forces, Wardley Mapping | Should we be in this market? |
| **0B** — Opportunity Sizing | Market quantification | TAM / SAM / SOM, Sensitivity Analysis | How big is the opportunity? |
| **0C** — Customer Discovery | Segmentation & needs | STP, Jobs-to-Be-Done, Four Forces of Progress | Who exactly, and what do they need? |
| **0D** — Value & Positioning | Differentiation & competitive strategy | Value Proposition Canvas, Blue Ocean (ERRC), Playing to Win | What's our right to win? |
| **0E** — Business Model | Revenue, economics & validation | Lean Canvas, AARRR Metrics, Opportunity Solution Trees | How does this become a business? |

### Phases 1–5: GSTIC Strategic Planning

With market understanding established, GSTIC builds the strategic and operational plan.

| Phase | Focus | Key Question |
|-------|-------|-------------|
| **G** — Goals | Objectives & KPIs | What does success look like? |
| **S** — Strategy | Audience & positioning | Who are we targeting and why will they choose us? |
| **T** — Tactics | Marketing & product mix | What levers do we pull? |
| **I** — Implementation | Roadmap & execution | How and when do we deliver? |
| **C** — Control | Measurement & iteration | How do we know it's working? |

Each phase produces a structured deliverable. Each deliverable traces back to the ones before it — tactics link to strategy, strategy links to goals, control covers every KPI.

## Quick Start

```bash
git clone https://github.com/jerimiahmertz/magstic-framework.git
cd magstic-framework
claude
```

Then say:

```
Run MAGSTIC for [your project name]
```

Claude Code loads `CLAUDE.md` when it starts in this folder. `CLAUDE.md` imports the kickoff prompt and the phase-transition rules, which run the phases in order. If you want to check, `/context` lists `CLAUDE.md` under memory files.

## What to Expect

- **Questions first.** Claude asks five to seven context questions once, then about six discovery questions per phase, each batch in one message. Answer what you can, or say "use your judgment" and Claude records its assumptions.
- **Bring evidence.** Phase 0 is only as good as its inputs. Put research, interview notes, metrics exports, and competitor material in `inputs/<project-slug>/` (the project name in lowercase with hyphens, e.g. `inputs/self-serve-trial/`; git-ignored) or give Claude paths and URLs. Every number in a Phase 0 deliverable carries a tag that says whether it came from you, a source, Claude's own knowledge, or an estimate.
- **A full run is long.** Ten phases, ten deliverables, and a review checkpoint after each. Market discovery alone is five phases; GSTIC alone is five. You can stop at any checkpoint and resume later.
- **Everything is saved.** Each phase is written to `output/<project-slug>/` once you confirm it. Start a new session in this folder and say "Show status" to pick up where you left off.

## How It Works

`CLAUDE.md` and the kickoff prompt tell Claude which of these files to read at each phase:

**Agents** (`agents/`) — Persona definitions that shape how Claude thinks during each phase. The Market Analyst evaluates opportunities with investor-grade rigor and must show its evidence. The Strategist pushes for specificity in goals and positioning. The Tactician builds executable plans. The Analyst designs measurement systems that drive decisions. They are plain Markdown files that Claude adopts in the main conversation, so every phase stays a back-and-forth with you.

**Templates** (`templates/`) — Structured output formats for each phase. These ensure deliverables are consistent, complete, and traceable across phases.

**Prompts** (`prompts/`) — Orchestration logic that controls phase sequencing, exit criteria, and context handoffs between agents.

**Examples** (`examples/`) — A complete, fictional GSTIC run compiled into a brief, showing the level of specificity expected.

**Saved deliverables** (`output/`) — Each phase is saved to `output/<project-slug>/` once you confirm it (`0a-market-attractiveness.md` through `5-control.md`, plus the compiled brief). You can stop, come back in a new session, revise a phase, or compile the brief later. `output/` and `inputs/` are git-ignored, so your plans and source material stay on your machine.

## Repo Structure

```
magstic-framework/
├── CLAUDE.md                          # Orchestration instructions (Claude Code loads this)
├── README.md
├── LICENSE
├── EVALUATION.md                      # Repository review and fix status (November 2026)
├── agents/
│   ├── market-analyst.md              # Phase 0: Market Discovery (all sub-phases)
│   ├── strategist.md                  # Phases 1–2: Goals & Strategy
│   ├── tactician.md                   # Phases 3–4: Tactics & Implementation
│   └── analyst.md                     # Phase 5: Control
├── templates/
│   ├── market-attractiveness.md       # Phase 0A output (PESTEL, Five Forces, Wardley Map)
│   ├── opportunity-sizing.md          # Phase 0B output (TAM/SAM/SOM)
│   ├── customer-discovery.md          # Phase 0C output (Segmentation, JTBD, Four Forces)
│   ├── value-positioning.md           # Phase 0D output (VPC, Blue Ocean, Playing to Win)
│   ├── business-model-canvas.md       # Phase 0E output (Lean Canvas, OST, Unit Economics)
│   ├── goals-worksheet.md             # Phase 1 output
│   ├── strategy-canvas.md             # Phase 2 output
│   ├── tactics-plan.md                # Phase 3 output
│   ├── implementation-roadmap.md      # Phase 4 output
│   ├── control-dashboard.md           # Phase 5 output
│   └── gstic-brief.md                 # Compiled brief (all phases)
├── prompts/
│   ├── kickoff.md                     # Entry orchestration & context gathering
│   └── phase-transitions.md           # Exit criteria and phase handoffs
├── examples/
│   └── tidewell-self-serve-gstic.md   # Worked example (fictional company, Phases 1–5)
├── inputs/                            # Your source material (created on first use, git-ignored)
└── output/                            # Your saved deliverables (created on first run, git-ignored)
```

## Usage Modes

| Command | What It Does |
|---------|-------------|
| `Run MAGSTIC for [project]` | All ten phases, 0A through Control |
| `Run market discovery for [project]` | Phase 0A through 0E only |
| `Run GSTIC for [project]` | Phases 1–5 only |
| `Run [phase] for [project]` | One phase — warns about missing earlier phases and states its assumptions |
| `Revise [phase]` | Re-open a saved phase, update it, and flag later phases that may need changes |
| `Show status` | List which phases are saved for the project and name the next one |
| `Compile brief` | Assemble the saved phases into one document |

## Tactics: Five Levers

Chernev's GSTIC defines seven tactics: product, service, brand, price, incentives, communication, and distribution. This framework uses five — **Product, Price, Incentives, Communication, Distribution** — to keep the Tactics phase manageable. Service decisions sit under Product, and brand decisions under Communication. To use all seven, add Service and Brand sections to `templates/tactics-plan.md` and `agents/tactician.md`, and update the Tactics exit criteria in `prompts/phase-transitions.md`.

## Integrated Frameworks

Built into the templates:

- **0A** — PESTEL, Porter's Five Forces, Wardley Mapping, competitive positioning map, weighted attractiveness scorecard
- **0B** — TAM/SAM/SOM (bottom-up, top-down, value-theory), sensitivity analysis
- **0C** — STP segmentation, Jobs-to-Be-Done, Four Forces of Progress
- **0D** — Value Proposition Canvas, Blue Ocean strategy canvas and ERRC grid, Playing to Win choice cascade
- **0E** — Lean Canvas, AARRR pirate metrics, Opportunity Solution Tree, riskiest-assumption ranking
- **Goals** — Objectives with KPIs, baselines, targets, owners, and non-goals
- **Strategy** — Value Proposition Canvas, Jobs-to-Be-Done, positioning statement, Hamilton Helmer's 7 Powers
- **Tactics** — Chernev marketing mix (five levers), RICE / ICE / MoSCoW prioritization
- **Implementation** — Stage-gated roadmap (Stage 0–3), RACI, dependency map, risk register
- **Control** — North Star metric with input, health, and counter-metrics, review cadence, experimentation design

Reach for these when the context calls for them: OKRs, Balanced Scorecard, AI Canvas, Growth Loops, HEART.

## Customization

- Add new agent personas in `agents/` for domain-specific roles (e.g., `growth-engineer.md` for PLG motions) and register them in the agent table in `CLAUDE.md` and the relevant step in `prompts/kickoff.md` — files are only read where those two point to them
- Add templates in `templates/` for recurring deliverable formats and list them in the template mapping in `CLAUDE.md`
- Modify `CLAUDE.md` to change orchestration behavior, output preferences, or framework integrations
- Add worked examples in `examples/` for team onboarding, with fictional companies and role titles instead of names

## License

[MIT](LICENSE)
