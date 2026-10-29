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

## Before Any Run: Check for Saved Work

Turn the project name into `<project-slug>` (lowercase kebab-case) and check `output/<project-slug>/`.

- **Phase files already exist:** read them, summarize where the project stands, and continue after the last saved phase (or with the phase the user asked for). Skip Step 0 — it was answered in the earlier session.
- **No phase files:** run the Step 0 for the user's mode below.
- **Single phase requested:** once context is in place (from saved files or Step 0), check which earlier phase files exist. If any are missing, use the out-of-sequence message in `prompts/phase-transitions.md` — name the missing phases, the risk, and the assumptions you'll make in their place. Then go to that phase's step below. For a Phase 0 sub-phase, use the full pipeline's Step 0.

Save each phase to the file named in its last step only after the user confirms it.

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
6. Once confirmed, save to `output/<project-slug>/0a-market-attractiveness.md`

### Phase 0B: Opportunity Sizing

1. Remain in `agents/market-analyst.md`
2. Read `templates/opportunity-sizing.md` — use as output structure
3. Reference the Market Attractiveness findings from 0A (`0a-market-attractiveness.md`)
4. Ask the Market Analyst's discovery questions for Phase 0B
5. Produce the Opportunity Sizing deliverable (TAM/SAM/SOM, sensitivity analysis)
6. Ask: "Ready to move to Customer Discovery, or refine the sizing?"
7. Once confirmed, save to `output/<project-slug>/0b-opportunity-sizing.md`

### Phase 0C: Customer Discovery

1. Remain in `agents/market-analyst.md`
2. Read `templates/customer-discovery.md` — use as output structure
3. Reference findings from 0A and 0B (`0a-market-attractiveness.md`, `0b-opportunity-sizing.md`)
4. Ask the Market Analyst's discovery questions for Phase 0C
5. Produce the Customer Discovery deliverable (Segmentation, JTBD, Four Forces)
6. Ask: "Ready to define positioning, or iterate on customer understanding?"
7. Once confirmed, save to `output/<project-slug>/0c-customer-discovery.md`

### Phase 0D: Value & Positioning

1. Remain in `agents/market-analyst.md`
2. Read `templates/value-positioning.md` — use as output structure
3. Reference all prior Phase 0 deliverables (`0a-` through `0c-`)
4. Ask the Market Analyst's discovery questions for Phase 0D
5. Produce the Value & Positioning deliverable (VPC, Strategy Canvas, ERRC, Playing to Win)
6. Ask: "Ready to model the business, or refine positioning?"
7. Once confirmed, save to `output/<project-slug>/0d-value-positioning.md`

### Phase 0E: Business Model

1. Remain in `agents/market-analyst.md`
2. Read `templates/business-model-canvas.md` — use as output structure
3. Reference all prior Phase 0 deliverables (`0a-` through `0d-`)
4. Ask the Market Analyst's discovery questions for Phase 0E
5. Produce the Business Model deliverable (Lean Canvas, OST, riskiest assumptions)
6. **Produce the Market Discovery Brief Summary** (Section 5 of the template)
7. Ask: "Market discovery is complete. Ready to move into GSTIC strategic planning, or iterate on the business model?"
8. Once confirmed, save to `output/<project-slug>/0e-business-model.md`

### Transition: Phase 0 → Phase 1

Use the Market Discovery Brief Summary from 0E as the context handoff. Deliver the 0E → Goals handoff message from `prompts/phase-transitions.md`. Then continue with Step 1 of "Run GSTIC" below — skip its Step 0, since context is already gathered.

---

## When the User Says "Run Market Discovery"

Run Step 0 and Phases 0A–0E from the full pipeline above (open with "Welcome to the GSTIC Strategic Planning Framework — Market Discovery." and drop the line about Phases 1–5). After 0E is saved, stop: present the Market Discovery Brief Summary and ask whether to continue into GSTIC now. If the user continues, go to Step 1 of "Run GSTIC" below — skip its Step 0.

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
3. If Phase 0 was completed, read the Market Discovery Brief Summary (Section 5 of `0e-business-model.md`) and use it to frame the Goals
4. Ask the Strategist's discovery questions for Goals
5. Produce the Goals deliverable
6. Ask: "Are you satisfied with the Goals, or would you like to iterate before we move to Strategy?"
7. Once confirmed, save to `output/<project-slug>/1-goals.md`

### Step 2: Strategy Phase

1. Remain in `agents/strategist.md` — Strategist persona continues
2. Read `templates/strategy-canvas.md` — use as output structure
3. Reference the Goals deliverable (`1-goals.md`)
4. Ask the Strategist's discovery questions for Strategy
5. Produce the Strategy deliverable
6. Ask: "Ready to move to Tactics, or do you want to refine the Strategy?"
7. Once confirmed, save to `output/<project-slug>/2-strategy.md`

### Step 3: Tactics Phase

1. Read `agents/tactician.md` — activate the Tactician persona
2. Read `templates/tactics-plan.md` — use as output structure
3. Reference the Goals and Strategy deliverables (`1-goals.md`, `2-strategy.md`)
4. Ask the Tactician's discovery questions for Tactics
5. Produce the Tactics deliverable
6. Ask: "Should we proceed to Implementation, or iterate on Tactics?"
7. Once confirmed, save to `output/<project-slug>/3-tactics.md`

### Step 4: Implementation Phase

1. Remain in `agents/tactician.md` — Tactician persona continues
2. Read `templates/implementation-roadmap.md` — use as output structure
3. Reference all prior deliverables (`1-goals.md` through `3-tactics.md`)
4. Ask the Tactician's discovery questions for Implementation
5. Produce the Implementation deliverable
6. Ask: "Ready for the Control phase, or do we need to adjust the plan?"
7. Once confirmed, save to `output/<project-slug>/4-implementation.md`

### Step 5: Control Phase

1. Read `agents/analyst.md` — activate the Analyst persona
2. Read `templates/control-dashboard.md` — use as output structure
3. Reference all prior deliverables (`1-goals.md` through `4-implementation.md`, especially Goals KPIs)
4. Ask the Analyst's discovery questions for Control
5. Produce the Control deliverable
6. Ask: "GSTIC plan is complete. Would you like to compile everything into a single brief, or iterate on any phase?"
7. Once confirmed, save to `output/<project-slug>/5-control.md`

---

## Final Compilation

If the user requests a compiled brief:
1. Read `templates/gstic-brief.md`
2. Read the saved phases from `output/<project-slug>/` — the Market Discovery Brief Summary from `0e-business-model.md` (if Phase 0 was run) and `1-goals.md` through `5-control.md`. If any are missing, say which and ask whether to compile anyway
3. Assemble all phase deliverables into a single document (Phase 0 summary + Phases 1–5)
4. Add an Executive Summary at the top (3-5 sentences)
5. Save as `output/<project-slug>/gstic-brief.md`

---

## Quick-Access Commands

Users can say these at any time:

| Command | Action |
|---------|--------|
| "Run full pipeline for [project]" | Phase 0A through Phase 5 |
| "Run market discovery for [project]" | Phase 0A through 0E |
| "Run GSTIC for [project]" | Full five-phase GSTIC run |
| "Run [phase] for [project]" | Single phase — warns about missing earlier phases and states assumptions |
| "Revise [phase]" | Re-open a saved phase, update its file, and flag later phases that may need changes |
| "Show status" | List which phase files exist in `output/<project-slug>/` and name the next phase |
| "Compile brief" | Assemble the saved phases into `output/<project-slug>/gstic-brief.md` |
