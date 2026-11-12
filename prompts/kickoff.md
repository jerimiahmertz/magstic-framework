# MAGSTIC Kickoff — Orchestration Prompt

## Entry Point Detection

| User Says | Entry Point | Run |
|-----------|-------------|-----|
| "Run MAGSTIC for [project]" or "Run full pipeline for [project]" | Phase 0A | All ten phases |
| "Run market discovery for [project]" | Phase 0A | 0A through 0E, then stop and offer Goals |
| "Run GSTIC for [project]" | Phase 1: Goals | Phases 1–5 |
| "Run [phase] for [project]" | That phase | One phase, after the out-of-sequence check |

"Show status", "Revise [phase]", and "Compile brief" are defined in `CLAUDE.md`.

## Step 0: Context Gathering

Do this once per project, before the first phase, whatever the entry point.

First check `output/<project-slug>/`. If it holds `0-context.md`, read it and any phase files, summarize where the project stands, and continue with the next incomplete phase (or the phase the user asked for) without re-asking Step 0. If phase files exist but `0-context.md` is missing, ask the Step 0 questions once and save them.

Otherwise, establish project context:

```
Welcome to the MAGSTIC Strategic Planning Framework.

Let's start with the basics:
1. What's the initiative or project name?
2. Give me a 2-3 sentence description of what this is about.
3. What's the time horizon? (e.g., a quarter, a fiscal year, multi-year)
4. What's the organizational context? (team size, company, division)
5. Is this B2B or B2C, and is it a new market entry or an optimization within a known market?
6. Has any market discovery already been done (research, interviews, sizing, competitor analysis)? Where does it live?
7. Do you have documents to ground this work (research reports, interview notes, metrics exports, competitor material)? Put them in a folder under inputs/ named after the project in lowercase with hyphens (I'll confirm the exact folder after you answer question 1), or give me paths or URLs.
```

Wait for the user's response before proceeding. Then state the project slug and the exact `inputs/<project-slug>/` path back to the user, and save the answers verbatim to `output/<project-slug>/0-context.md`. Carry these answers into every phase and never re-ask them. Use them to calibrate the depth and vocabulary of each phase.

For a GSTIC-only run: if the answer to question 6 is "none" or thin (no named target segment and no named alternative), say: "It sounds like we might benefit from running market discovery (Phase 0) first. Want to start there, or proceed directly to Goals?"

### Single-Phase Runs

If the user asked for one phase, check which earlier phase files exist in `output/<project-slug>/`. If any are missing, use the out-of-sequence message in `prompts/phase-transitions.md` — name the missing phases, the risk, and the assumptions you'll make in their place. Then go to that phase's step below.

---

## Phase 0: Market Discovery (0A–0E)

The Market Analyst persona (`agents/market-analyst.md`) stays active for all five phases. Read it once at 0A and keep its Evidence Rules in force. Every Phase 0 deliverable carries provenance tags and ends with its Sources table filled in.

### Phase 0A: Market Attractiveness

1. Read `agents/market-analyst.md` — activate the Market Analyst persona
2. Read `templates/market-attractiveness.md` — use as output structure
3. Ask the Market Analyst's discovery questions for Phase 0A, in one message, and wait
4. Gather evidence: read `inputs/<project-slug>/` and any paths or URLs given; when web tools are available, search for each named competitor, market figure, and regulation; note what you could not verify
5. Produce the Market Attractiveness deliverable (PESTEL, Five Forces, Wardley Map, Scorecard)
6. Check the 0A exit criteria in `prompts/phase-transitions.md` and print the checklist with ticks
7. Ask: **"Are you satisfied with the market assessment, or would you like to iterate before we size the opportunity?"**
8. Once confirmed, save to `output/<project-slug>/0a-market-attractiveness.md` and deliver the 0A → 0B handoff message

### Phase 0B: Opportunity Sizing

1. Remain in `agents/market-analyst.md`
2. Read `templates/opportunity-sizing.md` — use as output structure
3. Reference the Market Attractiveness findings (`0a-market-attractiveness.md`)
4. Ask the Market Analyst's discovery questions for Phase 0B, in one message, and wait
5. Gather evidence: industry reports, analogous markets, and the user's own customer and revenue data
6. Produce the Opportunity Sizing deliverable (TAM/SAM/SOM, sensitivity analysis)
7. Check the 0B exit criteria and print the checklist with ticks
8. Ask: **"Ready to move to Customer Discovery, or refine the sizing?"**
9. Once confirmed, save to `output/<project-slug>/0b-opportunity-sizing.md` and deliver the 0B → 0C handoff message

### Phase 0C: Customer Discovery

1. Remain in `agents/market-analyst.md`
2. Read `templates/customer-discovery.md` — use as output structure
3. Reference findings from 0A and 0B
4. Ask the Market Analyst's discovery questions for Phase 0C, in one message, and wait
5. Gather evidence: interview notes, surveys, support tickets, usage data, reviews of current alternatives
6. Produce the Customer Discovery deliverable (Segmentation, JTBD, Four Forces)
7. Check the 0C exit criteria and print the checklist with ticks
8. Ask: **"Ready to define positioning, or iterate on customer understanding?"**
9. Once confirmed, save to `output/<project-slug>/0c-customer-discovery.md` and deliver the 0C → 0D handoff message

### Phase 0D: Value & Positioning

1. Remain in `agents/market-analyst.md`
2. Read `templates/value-positioning.md` — use as output structure
3. Reference all prior Phase 0 deliverables
4. Ask the Market Analyst's discovery questions for Phase 0D, in one message, and wait
5. Gather evidence: competitor product pages, pricing pages, reviews, analyst comparisons
6. Produce the Value & Positioning deliverable (VPC, Strategy Canvas, ERRC, Playing to Win)
7. Check the 0D exit criteria and print the checklist with ticks
8. Ask: **"Ready to model the business, or refine positioning?"**
9. Once confirmed, save to `output/<project-slug>/0d-value-positioning.md` and deliver the 0D → 0E handoff message

### Phase 0E: Business Model

1. Remain in `agents/market-analyst.md`
2. Read `templates/business-model-canvas.md` — use as output structure
3. Reference all prior Phase 0 deliverables
4. Ask the Market Analyst's discovery questions for Phase 0E, in one message, and wait
5. Gather evidence: pricing benchmarks, cost data, analogous business models
6. Produce the Business Model deliverable (Lean Canvas, OST, riskiest assumptions), including the **Market Discovery Brief Summary** (section 5 of the template)
7. Check the 0E exit criteria and print the checklist with ticks
8. Ask: **"Are you satisfied with the business model, or would you like to iterate before we close market discovery?"**
9. Once confirmed, save to `output/<project-slug>/0e-business-model.md`

### Transition: Phase 0 → Phase 1

- **Market discovery only:** stop here. Present the Market Discovery Brief Summary (section 5 of `0e-business-model.md`) and deliver the 0E → Goals handoff message from `prompts/phase-transitions.md` with its market-discovery closing line, which asks whether to continue into Goals.
- **Full pipeline:** deliver the 0E → Goals handoff message from `prompts/phase-transitions.md` with its full-pipeline closing line, then continue with Step 1 below. Do not repeat Step 0.

---

## Phases 1–5: GSTIC

### Step 1: Goals Phase

1. Read `agents/strategist.md` — activate the Strategist persona
2. Read `templates/goals-worksheet.md` — use as output structure
3. If Phase 0 was run, reference the Market Discovery Brief Summary (`0e-business-model.md`, section 5); its riskiest assumptions should shape the objectives
4. Ask the Strategist's discovery questions for Goals, in one message, and wait
5. Produce the Goals deliverable
6. Check the Goals exit criteria and print the checklist with ticks
7. Ask: **"Are you satisfied with the Goals, or would you like to iterate before we move to Strategy?"**
8. Once confirmed, save to `output/<project-slug>/1-goals.md` and deliver the Goals → Strategy handoff message

### Step 2: Strategy Phase

1. Remain in `agents/strategist.md` — Strategist persona continues
2. Read `templates/strategy-canvas.md` — use as output structure
3. Reference the Goals deliverable (`1-goals.md`). If Phase 0 was run, take segments, JTBD, alternatives, switching forces, and moat from `0c-customer-discovery.md` and `0d-value-positioning.md`, record only what changed and why, and drop the discovery questions they already answer
4. Ask the Strategist's remaining discovery questions for Strategy, in one message, and wait
5. Produce the Strategy deliverable
6. Check the Strategy exit criteria and print the checklist with ticks
7. Ask: **"Ready to move to Tactics, or do you want to refine the Strategy?"**
8. Once confirmed, save to `output/<project-slug>/2-strategy.md` and deliver the Strategy → Tactics handoff message

### Step 3: Tactics Phase

1. Read `agents/tactician.md` — activate the Tactician persona
2. Read `templates/tactics-plan.md` — use as output structure
3. Reference the Goals and Strategy deliverables (`1-goals.md`, `2-strategy.md`)
4. Ask the Tactician's discovery questions for Tactics, in one message, and wait
5. Produce the Tactics deliverable
6. Check the Tactics exit criteria and print the checklist with ticks
7. Ask: **"Should we proceed to Implementation, or iterate on Tactics?"**
8. Once confirmed, save to `output/<project-slug>/3-tactics.md` and deliver the Tactics → Implementation handoff message

### Step 4: Implementation Phase

1. Remain in `agents/tactician.md` — Tactician persona continues
2. Read `templates/implementation-roadmap.md` — use as output structure
3. Reference all prior deliverables
4. Ask the Tactician's discovery questions for Implementation, in one message, and wait
5. Produce the Implementation deliverable
6. Check the Implementation exit criteria and print the checklist with ticks
7. Ask: **"Ready for the Control phase, or do we need to adjust the plan?"**
8. Once confirmed, save to `output/<project-slug>/4-implementation.md` and deliver the Implementation → Control handoff message

### Step 5: Control Phase

1. Read `agents/analyst.md` — activate the Analyst persona
2. Read `templates/control-dashboard.md` — use as output structure
3. Reference all prior deliverables (especially Goals KPIs)
4. Ask the Analyst's discovery questions for Control, in one message, and wait
5. Produce the Control deliverable
6. Check the Control exit criteria and print the checklist with ticks
7. Ask: **"Are you satisfied with the Control plan, or would you like to iterate before we close the plan?"**
8. Once confirmed, save to `output/<project-slug>/5-control.md` and deliver the completion message

---

## Final Compilation

If the user requests a compiled brief:

1. Read `templates/gstic-brief.md`
2. Read the saved phase files from `output/<project-slug>/` — if any are missing, say which and ask whether to compile anyway
3. Section 0: insert the six subsections of section 5 (Market Attractiveness through Top Risks) of `0e-business-model.md` verbatim, without the `## 5.` heading line or its italic note; if market discovery was not run, use the one-line note from the template
4. Insert each GSTIC phase file (`1-goals.md` through `5-control.md`) in its section
5. Add an Executive Summary at the top (3–5 sentences); fill in the Phase Completion Tracker, the Key Decisions Log (from decisions and assumptions recorded in the phase files), the Stakeholder Map (from the RACI in `4-implementation.md`), and the Glossary (terms used across phases); delete any appendix table that has nothing to hold
6. Save as `output/<project-slug>/magstic-brief.md`
