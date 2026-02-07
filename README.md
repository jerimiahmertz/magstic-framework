# GSTIC Strategic Planning Framework

A structured, AI-driven strategic planning framework designed to run interactively via [Claude Code](https://docs.anthropic.com/en/docs/claude-code). GSTIC provides a complete pipeline from market discovery through execution measurement — ten phases across two stages.

## What is GSTIC?

### Stage 1: Builder PM Stack (Phase 0) — Market Discovery

Before you build strategy, validate the market. Phase 0 uses five sub-phases to determine whether an opportunity is worth pursuing and how to position within it.

| Phase | Focus | Frameworks Used | Key Question |
|-------|-------|----------------|-------------|
| **0A** — Market Attractiveness | Macro forces, competitive dynamics, value chain | PESTEL, Porter's Five Forces, Wardley Mapping | Should we be in this market? |
| **0B** — Opportunity Sizing | Market quantification | TAM / SAM / SOM, Sensitivity Analysis | How big is the opportunity? |
| **0C** — Customer Discovery | Segmentation & needs | STP, Jobs-to-Be-Done, Four Forces of Progress | Who exactly, and what do they need? |
| **0D** — Value & Positioning | Differentiation & competitive strategy | Value Proposition Canvas, Blue Ocean (ERRC), Playing to Win | What's our right to win? |
| **0E** — Business Model | Revenue, economics & validation | Lean Canvas, AARRR Metrics, Opportunity Solution Trees | How does this become a business? |

### Stage 2: GSTIC Strategic Planning (Phases 1–5)

With market understanding established, GSTIC builds the strategic and operational plan.

| Phase | Focus | Key Question |
|-------|-------|-------------|
| **G** — Goals | Objectives & KPIs | What does success look like? |
| **S** — Strategy | Audience & positioning | Who are we targeting and why us? |
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

| Command | What It Does |
|---------|-------------|
| `Run full pipeline for [project]` | All 10 phases: Market Discovery → GSTIC |
| `Run market discovery for [project]` | Phase 0A–0E only |
| `Run GSTIC for [project]` | Phases 1–5 only |
| `Run Phase 0C for [project]` | Single phase (warns if prerequisites incomplete) |

Claude Code reads `CLAUDE.md` on entry and orchestrates the workflow automatically.

## How It Works

The framework uses four components that Claude Code picks up from the repo:

**Agents** (`agents/`) — Persona definitions that shape how Claude thinks during each phase. The Market Analyst evaluates opportunities with investor-grade rigor. The Strategist pushes for specificity in goals and positioning. The Tactician builds executable plans. The Analyst designs measurement systems that drive decisions.

**Templates** (`templates/`) — Structured output formats for each phase. These ensure deliverables are consistent, complete, and traceable across phases.

**Prompts** (`prompts/`) — Orchestration logic that controls phase sequencing, transition criteria, and context handoffs between agents.

**Examples** (`examples/`) — Worked examples showing completed GSTIC briefs for reference.

## Repo Structure

```
magstic-framework/
├── CLAUDE.md                          # Orchestration instructions (Claude Code reads this)
├── README.md
│
├── agents/
│   ├── market-analyst.md              # Phase 0: Market Discovery (all sub-phases)
│   ├── strategist.md                  # Phases 1–2: Goals & Strategy
│   ├── tactician.md                   # Phases 3–4: Tactics & Implementation
│   └── analyst.md                     # Phase 5: Control
│
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
│   └── gstic-brief.md                 # Full compiled brief (all phases)
│
├── prompts/
│   ├── kickoff.md                     # Entry orchestration & context gathering
│   └── phase-transitions.md           # Phase handoff logic & exit criteria
│
└── examples/
    └── warm-leads-gstic.md            # Worked example
```

## Usage Modes

| Command | What It Does |
|---------|-------------|
| `Run full pipeline for [project]` | All 10 phases sequentially |
| `Run market discovery for [project]` | Phase 0A through 0E |
| `Run GSTIC for [project]` | Phase 1 through 5 |
| `Run the Strategy phase for [project]` | Single phase (warns if prerequisites incomplete) |
| `Revise the Tactics` | Re-enter a completed phase to refine |
| `Show status` | See which phases are done |
| `Compile brief` | Assemble all phases into one document |
| `Skip to [phase]` | Jump ahead with dependency warning |

## Integrated Frameworks

GSTIC phases pull in complementary frameworks depending on context:

### Phase 0: Builder PM Stack
* **0A** — PESTEL, Porter's Five Forces, Wardley Mapping, Competitive Positioning
* **0B** — TAM/SAM/SOM (Bottom-Up, Top-Down, Value-Theory), Sensitivity Analysis
* **0C** — STP Segmentation, Jobs-to-Be-Done (JTBD), Four Forces of Progress
* **0D** — Value Proposition Canvas, Blue Ocean Strategy Canvas, ERRC Grid, Playing to Win Choice Cascade
* **0E** — Lean Canvas, AARRR Pirate Metrics, Opportunity Solution Trees

### Phases 1–5: GSTIC
* **Goals** — OKRs, Balanced Scorecard
* **Strategy** — Hamilton Helmer's 7 Powers, Jobs-to-Be-Done, AI Canvas
* **Tactics** — Chernev Marketing Mix, Growth Loops, RICE/ICE Scoring
* **Implementation** — Phased Roadmapping (P0-P3), RACI, Risk Registers
* **Control** — North Star Metric, HEART Framework, AARRR, Experimentation Design

## Customization

- Add new agent personas in `agents/` for domain-specific roles (e.g., `growth-engineer.md` for PLG motions)
- Add templates in `templates/` for recurring deliverable formats
- Modify `CLAUDE.md` to change orchestration behavior, output preferences, or framework integrations
- Add worked examples in `examples/` for team onboarding

## License

MIT
