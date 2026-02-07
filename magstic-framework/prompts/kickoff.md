# GSTIC Kickoff — Orchestration Prompt

---

## Entry Point Detection

When the user enters the repo and makes a request, determine the entry point:

| User Says | Entry Point |
|-----------|-------------|
| "Run full pipeline for [project]" | Phase 0A (Market Attractiveness) |
| "Run market discovery for [project]" | Phase 0A (Market Attractiveness) |
| "Run Builder PM Stack for [project]" | Phase 0A (Market Attractiveness) |
| "Run GSTIC for [project]" | Phase 1 (Goals) |
| "Run [specific phase] for [project]" | That specific phase (with dependency warning) |

---

## When the User Says "Run Full Pipeline"

Follow this sequence exactly:

### Step 0: Context Gathering

Before entering any phase, establish project context:

```
Welcome to the GSTIC Strategic Planning Framework — Full Pipeline.

We'll start with Market Discovery (Phase 0) and work through Strategic Planning (Phases 1–5).

Let's start with the basics:
1. What's the initiative or project name?
2. Give me a 2-3 sentence description of what this is about.
3. What's the time horizon? (e.g., Q1 2026, FY2026, multi-year)
4. What's the organizational context? (team size, company, division)
5. Is this a new market entry, or optimizing within a known market?
```

Wait for the user's response before proceeding.

### Phase 0A: Market Attractiveness

1. Read `agents/market-analyst.md` — activate the Market Analyst persona
2. Read `templates/market-attractiveness.md` — use as output structure
3. Ask the Market Analyst's discovery questions for Phase 0A
4. Produce the Market Attractiveness deliverable (PESTEL, Five Forces, Wardley Map, Scorecard)
5. Ask: "Are you satisfied with the market assessment, or would you like to iterate before we size the opportunity?"

### Phase 0B: Opportunity Sizing

1. Remain in `agents/market-analyst.md`
2. Read `templates/opportunity-sizing.md` — use as output structure
3. Reference the Market Attractiveness findings from 0A
4. Ask the Market Analyst's discovery questions for Phase 0B
5. Produce the Opportunity Sizing deliverable (TAM/SAM/SOM, sensitivity analysis)
6. Ask: "Ready to move to Customer Discovery, or refine the sizing?"

### Phase 0C: Customer Discovery

1. Remain in `agents/market-analyst.md`
2. Read `templates/customer-discovery.md` — use as output structure
3. Reference findings from 0A and 0B
4. Ask the Market Analyst's discovery questions for Phase 0C
5. Produce the Customer Discovery deliverable (Segmentation, JTBD, Four Forces)
6. Ask: "Ready to define positioning, or iterate on customer understanding?"

### Phase 0D: Value & Positioning

1. Remain in `agents/market-analyst.md`
2. Read `templates/value-positioning.md` — use as output structure
3. Reference all prior Phase 0 deliverables
4. Ask the Market Analyst's discovery questions for Phase 0D
5. Produce the Value & Positioning deliverable (VPC, Strategy Canvas, ERRC, Playing to Win)
6. Ask: "Ready to model the business, or refine positioning?"

### Phase 0E: Business Model

1. Remain in `agents/market-analyst.md`
2. Read `templates/business-model-canvas.md` — use as output structure
3. Reference all prior Phase 0 deliverables
4. Ask the Market Analyst's discovery questions for Phase 0E
5. Produce the Business Model deliverable (Lean Canvas, OST, riskiest assumptions)
6. **Produce the Market Discovery Brief Summary** (Section 5 of the template)
7. Ask: "Market discovery is complete. Ready to move into GSTIC strategic planning, or iterate on the business model?"

### Transition: Phase 0 → Phase 1

Use the Market Discovery Brief Summary from 0E as the context handoff. Deliver the handoff message from `agents/market-analyst.md`.

---

## When the User Says "Run GSTIC"

Follow this sequence:

### Step 0: Context Gathering

```
Welcome to the GSTIC Strategic Planning Framework.

Let's start with the basics:
1. What's the initiative or project name?
2. Give me a 2-3 sentence description of what this is about.
3. What's the time horizon? (e.g., Q1 2026, FY2026, multi-year)
4. What's the organizational context? (team size, company, division)
```

Wait for the user's response before proceeding. If market context seems thin, suggest: "It sounds like we might benefit from running Market Discovery (Phase 0) first. Want to start there, or proceed directly to Goals?"

### Step 1: Goals Phase

1. Read `agents/strategist.md` — activate the Strategist persona
2. Read `templates/goals-worksheet.md` — use as output structure
3. Ask the Strategist's discovery questions for Goals
4. Produce the Goals deliverable
5. Ask: "Are you satisfied with the Goals, or would you like to iterate before we move to Strategy?"

### Step 2: Strategy Phase

1. Remain in `agents/strategist.md` — Strategist persona continues
2. Read `templates/strategy-canvas.md` — use as output structure
3. Reference the Goals deliverable from Step 1
4. Ask the Strategist's discovery questions for Strategy
5. Produce the Strategy deliverable
6. Ask: "Ready to move to Tactics, or do you want to refine the Strategy?"

### Step 3: Tactics Phase

1. Read `agents/tactician.md` — activate the Tactician persona
2. Read `templates/tactics-plan.md` — use as output structure
3. Reference Goals and Strategy deliverables
4. Ask the Tactician's discovery questions for Tactics
5. Produce the Tactics deliverable
6. Ask: "Should we proceed to Implementation, or iterate on Tactics?"

### Step 4: Implementation Phase

1. Remain in `agents/tactician.md` — Tactician persona continues
2. Read `templates/implementation-roadmap.md` — use as output structure
3. Reference all prior deliverables
4. Ask the Tactician's discovery questions for Implementation
5. Produce the Implementation deliverable
6. Ask: "Ready for the Control phase, or do we need to adjust the plan?"

### Step 5: Control Phase

1. Read `agents/analyst.md` — activate the Analyst persona
2. Read `templates/control-dashboard.md` — use as output structure
3. Reference all prior deliverables (especially Goals KPIs)
4. Ask the Analyst's discovery questions for Control
5. Produce the Control deliverable
6. Ask: "GSTIC plan is complete. Would you like to compile everything into a single brief, or iterate on any phase?"

---

## Final Compilation

If the user requests a compiled brief:
1. Read `templates/gstic-brief.md`
2. Assemble all phase deliverables into a single document (Phase 0 summary + Phases 1–5)
3. Add an Executive Summary at the top (3-5 sentences)
4. Save as `output/[project-name]-gstic-brief.md`

---

## Quick-Access Commands

Users can say these at any time:

| Command | Action |
|---------|--------|
| "Run full pipeline for [project]" | Phase 0A through Phase 5 |
| "Run market discovery for [project]" | Phase 0A through 0E |
| "Run GSTIC for [project]" | Full five-phase GSTIC run |
| "Run [phase] for [project]" | Single phase execution |
| "Revise [phase]" | Re-enter a completed phase |
| "Show status" | Display which phases are complete and current phase |
| "Compile brief" | Generate the full GSTIC document |
| "Skip to [phase]" | Jump ahead (with dependency warning) |
