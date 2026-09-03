# MAGSTIC Framework Repository Evaluation

**Repository:** jerimiahmertz/magstic-framework, branch `master` at 5d1c8d3, evaluated 2026-09-03.
**Scope:** every file in the repo (24 files), read in full, plus the sibling repo jerimiahmertz/gstic-framework (v1) for comparison.
**Method:** eight independent review lenses produced 144 candidate findings; each was checked by two adversarial verifiers (factual accuracy and materiality; correctness of the fix) and 58 were refuted and dropped. A completeness critic then added three lenses (evidence acquisition, example consistency, rendering and accessibility) whose 40 candidates went through the same check, 15 surviving. The 126 survivors are merged below into 66 findings. Finding ids in parentheses (F1, G12) refer to the raw candidates.

## Summary

The framework does not run as documented. The README Quick Start launches `claude` in the clone root, but the only CLAUDE.md sits one level down at magstic-framework/CLAUDE.md, so a user who follows the three documented lines gets a plain Claude Code session with no personas, templates or phase sequencing. Even after that is fixed, CLAUDE.md sends only the GSTIC-only mode to prompts/kickoff.md and nothing at all to prompts/phase-transitions.md, which is a v2 regression (v1 listed both as top-level instructions), and nothing is ever saved per phase, so `Show status`, `Revise` and `Compile brief` rest on chat memory that compaction erases. Nine files (three agent personas, five GSTIC templates, the worked example) have every heading, table and rule stripped; word-identical intact copies exist in the sibling public repo jerimiahmertz/gstic-framework, so restoration is a file copy, not a rebuild. The example also names a real company, its internal systems and a named employee in a public MIT repo. Below these, the methodology is largely sound but has fidelity gaps: five of Chernev's seven Ts, no collaborators, company value or context in Strategy, no kill criteria, and no step that tells the Market Analyst to gather evidence before filling 25 tables. What is already good: the five Phase 0 templates and agents/market-analyst.md are intact, specific and well structured; phase-transitions.md holds exactly the right exit criteria and handoff scripts; and the end-to-end traceability design (KPI coverage check, traceability matrix, phase tracker) is a strong foundation that only needs to be wired up.

## Fix first

1. Flatten the repository so CLAUDE.md, agents/, prompts/, templates/ and examples/ sit at the clone root, and delete the zip, both .DS_Store files and the nested README in the same commit (critical, S). Every other fix in this report is inert until Claude Code actually loads CLAUDE.md.
2. Copy the nine stripped files from /tmp/gstic-v1/gstic-framework/ over the stripped copies; text is word-for-word identical and only markup differs (high, S).
3. Rewrite examples/warm-leads-gstic.md with a fictional company, generic system names and roles instead of people, then purge the original from history with git filter-repo in this repo and in gstic-framework (high, M).
4. Wire the orchestration: add `@prompts/kickoff.md` and `@prompts/phase-transitions.md` imports to CLAUDE.md so every mode reads them, and make exit criteria a gate on every transition (high, S).
5. Add per-phase persistence: output/<slug>/ with one file per phase and a STATUS.md that `Show status`, `Revise`, `Skip to` and `Compile brief` read from (high, M).
6. Give the Market Analyst an evidence step and a provenance rule so ratings, market sizes and competitor claims carry a source tag or an explicit "estimate" label instead of being invented under an "Evidence" heading (high, M).
7. One hygiene and naming commit: .gitignore, LICENSE, .editorconfig, a MAGSTIC/GSTIC naming pass, a single command table, and a tag once the restored files are in (medium, S).

### Restore and flatten: the commands

The nine stripped files are word-for-word identical to the copies in the sibling repo `jerimiahmertz/gstic-framework`; only the Markdown markup differs. Run this from the clone root on a fresh branch:

```bash
git clone --depth 1 https://github.com/jerimiahmertz/gstic-framework /tmp/gstic-v1

# 1. Restore the nine stripped files (text unchanged, markup returns)
for f in agents/strategist.md agents/tactician.md agents/analyst.md \
         templates/goals-worksheet.md templates/strategy-canvas.md templates/tactics-plan.md \
         templates/implementation-roadmap.md templates/control-dashboard.md \
         examples/warm-leads-gstic.md; do
  cp "/tmp/gstic-v1/gstic-framework/$f" "magstic-framework/$f"
done

# 2. Flatten so CLAUDE.md sits in the directory where `claude` is launched
git mv magstic-framework/CLAUDE.md magstic-framework/agents magstic-framework/prompts \
       magstic-framework/templates magstic-framework/examples .
git rm -r magstic-framework            # removes the nested README.md and .DS_Store
git rm magstic-framework.zip .DS_Store

# 3. Ignore OS junk, archives and generated strategy documents
printf '.DS_Store\n._*\n*.zip\noutput/*\n!output/.gitkeep\n' > .gitignore
mkdir -p output && touch output/.gitkeep

# 4. Verify the restore
grep -c '^#' agents/strategist.md agents/tactician.md agents/analyst.md
#   expect 14, 15, 13
grep -c '|' templates/{goals-worksheet,strategy-canvas,tactics-plan,implementation-roadmap,control-dashboard}.md
#   expect 24, 49, 34, 83, 69

git add -A && git commit -m "Restore stripped GSTIC files from v1 and flatten repo layout"
```

The restored example still contains the real company, system and person names (finding 10). Rewrite it before the next push, and decide separately whether to purge the old history.

## Findings by theme

### Works in Claude Code

**1. CLAUDE.md sits one directory below the clone root, so the Quick Start launches Claude with no instructions** (critical, S; F1, F18, F78, F94, F108)
- Evidence: README.md:36-38 says `cd magstic-framework` then `claude`; README.md:50 says "Claude Code reads `CLAUDE.md` on entry". `git ls-files` shows the only copy at magstic-framework/CLAUDE.md; the clone root holds README.md, magstic-framework.zip and .DS_Store. Claude Code loads CLAUDE.md from the working directory and its parents; a subdirectory copy loads only when Claude reads a file there, and it is then summarised away at compaction.
- Impact: A user who follows the README gets a plain session; "Run full pipeline for X" produces a generic answer. `unzip -l` shows the archive is flat, so zip users get a working layout and git users do not.
- Fix: Tested flatten: `git mv magstic-framework/CLAUDE.md magstic-framework/agents magstic-framework/templates magstic-framework/prompts magstic-framework/examples .` then `git rm -r magstic-framework magstic-framework.zip .DS_Store`. Add .gitignore with `.DS_Store`, `*.zip`, `output/*`, `!output/.gitkeep` and commit output/.gitkeep. No path edits are needed inside CLAUDE.md or prompts/. Add under README.md:50: "Run `/context` and confirm CLAUDE.md appears under Memory files." Fallback only if flattening is refused: change README.md:37 to `cd magstic-framework/magstic-framework` and redraw the tree.

**2. CLAUDE.md tells only the GSTIC-only mode to read kickoff.md; Full Pipeline, Market Discovery and Single Phase never reach the orchestration script** (high, S; F2, F20, F79, F98)
- Evidence: CLAUDE.md:43 "1. Read the kickoff prompt at `prompts/kickoff.md`" appears only under "### GSTIC Only" (line 42). The Full Pipeline (37-40), Builder PM Stack Only (47-49) and Single Phase (51-54) sections have no read step. No `@` imports exist. The v1 CLAUDE.md in gstic-framework carried "Read the kickoff prompt" as a top-level instruction; v2 moved it into one branch. README.md:54 claims four components are "picked up from the repo", but only CLAUDE.md is auto-loaded.
- Impact: For the headline command Claude is never told to open kickoff.md, so the Step 0 questions (kickoff.md:27-40), the per-phase "Read `agents/market-analyst.md`" steps (44-45) and the iteration prompts are reached only by accident. Results vary run to run.
- Fix: After flattening, add under "## How to Use This Framework" (CLAUDE.md:33) a "## Required reading" block with `@prompts/kickoff.md` and `@prompts/phase-transitions.md` outside backticks (about 17 KB at launch), plus: "Before any phase, use the Read tool on the agent file and the template file named in the Read steps of prompts/kickoff.md for that phase. Never produce a deliverable from memory of the framework." Point each mode section at its kickoff.md section, delete line 43, and do not import agents/ or templates/ (89 KB). Correct README.md:54.

**3. prompts/phase-transitions.md is referenced by nothing; a v2 regression that unplugs every exit criterion and handoff** (high, S; F3, F19)
- Evidence: `grep -rn phase-transitions --include=*.md .` hits only README.md:92 and the nested README. The file holds the only per-phase Exit Criteria (phase-transitions.md:21-28 through 211-218), nine of ten Handoff Messages, and the "Handling Out-of-Sequence Requests" scripts (226-241) that back CLAUDE.md:142 and :145. The v1 CLAUDE.md listed "Follow phase transitions defined in prompts/phase-transitions.md" at top level; v2 dropped it.
- Impact: CLAUDE.md:101 says "Each phase must produce a deliverable before transitioning", but the checklist that defines done never enters context. Phases can close without an ERRC Create row or SOM Y1/Y3/Y5 although phase-transitions.md:87 and :45 require them. Skip and Revise behaviour is improvised.
- Fix: Import it (finding 2) and extend Orchestration Behavior item 2: "A phase is complete only when every Exit Criteria checkbox for that phase in prompts/phase-transitions.md is satisfied and the user has confirmed; print the checklist with ticks, then deliver that phase's Handoff Message with placeholders filled." Make it the single source of handoff text (finding 21). Long term, split it into per-phase runbooks under .claude/skills/magstic/phases/.

**4. Nothing is saved per phase; Show status, Revise, Skip and Compile depend on chat memory that does not survive compaction or a new session** (high, M; F4, F22, F83, F84, F97)
- Evidence: kickoff.md:163 "4. Save as `output/[project-name]-gstic-brief.md`" is the only write instruction in the repo. CLAUDE.md:143 "`Show status` | Display which phases are complete" has no backing store. gstic-brief.md:39 "[Paste completed Goals Worksheet content here]". phase-transitions.md:234 promises "I'll propagate any updates to downstream phases" with no steps. No output/ directory and no .gitignore exist.
- Impact: A full run reads about 89 KB of framework files and generates ten long deliverables, so auto-compaction fires mid-run and earlier deliverables survive only as summaries. `Compile brief` pastes reconstructed text; `Show status` after relaunching returns nothing; a brief written to an unignored output/ lands in the working tree of a public repo clone.
- Fix: Add "## Persistence (mandatory)" to CLAUDE.md: all work lives in output/<project-slug>/ as STATUS.md, 0a-market-attractiveness.md, 0b-opportunity-sizing.md, 0c-customer-discovery.md, 0d-value-positioning.md, 0e-business-model.md, 1-goals.md, 2-strategy.md, 3-tactics.md, 4-implementation.md, 5-control.md, brief.md. STATUS.md carries the Phase Completion Tracker from templates/gstic-brief.md:85-96 plus a File column and a 150-word carry-forward block per phase. Write the phase file and update STATUS.md when the user confirms. `Show status` reads STATUS.md; `Revise` edits the file and marks downstream rows needs review; `Skip to` checks prerequisites in STATUS.md; `Compile brief` builds brief.md from the files. Add: "If you cannot see the full text of an earlier deliverable in this conversation, read it from output/<slug>/ before using it." Change kickoff.md:163 to output/<slug>/brief.md. Pair with a SessionStart hook (matcher `compact`) in .claude/settings.json that prints output/*/STATUS.md.

**5. The full-pipeline read set is about 95 KB and 8,120 lines, of which 5,398 lines are blank padding from the stripped files** (medium, M; F85)
- Evidence: `cat CLAUDE.md prompts/*.md agents/*.md templates/*.md | wc` gives 95,341 bytes and 8,120 lines. implementation-roadmap.md is 1,500 lines with 167 non-blank; control-dashboard.md 1,377 with 179 non-blank. The Read tool prefixes every line with a number, so blank lines still cost tokens.
- Impact: A full run lands around 80-120k tokens before answers, so compaction is guaranteed mid-pipeline and takes the persona instructions with it.
- Fix: Copy the nine intact files from gstic-framework (implementation-roadmap.md drops from 1,500 to 156 lines). Add to kickoff.md: "Do not re-quote earlier deliverables in chat; read output/<slug>/<phase>.md when a detail is needed." Add the compact hook above and an optional "# Compact instructions" section in CLAUDE.md asking the summariser to keep phase status, KPIs and open assumptions.

**6. Commands are free-text phrases with inconsistent triggers and no slash-command surface** (low, M; F6)
- Evidence: CLAUDE.md:37 keys the full pipeline on "Run full GSTIC" while CLAUDE.md:138, kickoff.md:11 and README.md:45 use "Run full pipeline for [project]". "Run GSTIC" (CLAUDE.md:42) means Phases 1-5, one word away from "Run full GSTIC".
- Impact: No autocomplete or `/help` listing; the near-collision can start a run at Phase 1 instead of 0A.
- Fix: Change CLAUDE.md:37 to `### Full Pipeline ("Run full pipeline for [project]")`. Optionally add skills at the repo root under .claude/skills/ (magstic, magstic-phase, magstic-status, magstic-revise, magstic-compile), each pointing at existing kickoff.md and phase-transitions.md sections, and pick one invocation model (user-only with `disable-model-invocation: true`, or model-callable).

### Content integrity

**7. Five GSTIC templates have every heading, table and rule stripped; intact copies exist in gstic-framework** (high, S; F9)
- Evidence: `grep -c '|'` is 0 in goals-worksheet.md, strategy-canvas.md, tactics-plan.md, implementation-roadmap.md and control-dashboard.md. control-dashboard.md:187-205 is the header "Metric / Definition / Source / Baseline / Target / Owner / Cadence" followed by 89 blank lines. implementation-roadmap.md:1259-1268 Risk Register row 1 is "1" / "H/M/L" / "H/M/L" out of seven columns. Line counts: goals 483 with 77 non-blank, strategy 868/135, tactics 835/104, implementation 1500/167, control 1377/179.
- Impact: kickoff.md tells Claude to "use as output structure", but a model cannot tell H2 from H3, cannot know the Input, Health and Counter metric tables had 3, 2 and 2 rows, and will invent row counts and column membership. Each phase costs about five times the tokens it should. No prose was lost; the structure was.
- Fix: Copy, do not rebuild: `cp /tmp/gstic-v1/gstic-framework/templates/{goals-worksheet,strategy-canvas,tactics-plan,implementation-roadmap,control-dashboard}.md magstic-framework/templates/`. Verify `grep -c '|'` gives 24/49/34/83/69 and `grep -c '^#'` gives 13/15/17/14/21. Then apply v2 edits on top: the duplicate "[Dimension A] →" header at restored strategy-canvas.md:52 (v1 has the same bug), header and footer standardisation, Action Trigger columns, kill criteria. Delete or regenerate magstic-framework.zip, which carries the stripped copies.

**8. Three GSTIC agent files (strategist, tactician, analyst) are stripped the same way** (high, S; F10, F86)
- Evidence: agents/strategist.md:25-27 "You are a" / "Senior Strategy Advisor" / "with deep expertise" split across three lines. analyst.md:301-378 is the four-column Review Cadence table flattened to one word per line. tactician.md:129-196 is the flattened Dimension / Key Question table. `grep -c '^---'` is 0 in all three versus 8 in market-analyst.md. Non-blank over total: strategist 64/323, tactician 85/437, analyst 93/479.
- Impact: Every question and check survives, but the Phase 1 and Phase 2 boundaries (strategist.md:51 and :178) are invisible, kickoff.md:115 "Ask the Strategist's discovery questions for Goals" has no heading to find, and Phases 1-5 present a visibly worse instruction surface than Phase 0.
- Fix: `cp /tmp/gstic-v1/gstic-framework/agents/{strategist,tactician,analyst}.md magstic-framework/agents/`. The normalised diff is empty, so no v2 text edits are needed. Verify `grep -c '^|'` gives 0/7/16 and `grep -n ' $'` gives nothing. Do not re-author them in market-analyst.md style; that would rename headings the originals never had.

**9. The worked example is stripped, is a summary that calls itself a completed run, fails the framework's own exit criteria, and is never read by any instruction** (medium, S to retitle, L to complete; F11, F42, F67, F93, F96)
- Evidence: examples/warm-leads-gstic.md:16 "This example demonstrates a completed GSTIC run" versus line 183 "Phase 3: Tactics (Summary)", 293, 385 and line 423 "This example is illustrative." README.md:62 promises "completed GSTIC briefs". `wc -w` is 525. The Tactics table (217-275) has no Price row although phase-transitions.md:173 requires all five dimensions; there is no traceability matrix, prioritisation method, RACI, risk register or KPI coverage check. `grep -rn 'examples/' CLAUDE.md prompts agents` returns nothing.
- Impact: The only calibration artefact teaches summary depth where the templates demand tables, covers nothing for Phase 0, and is invisible to Claude anyway.
- Fix: Copy the intact file from gstic-framework/examples/warm-leads-gstic.md, anonymise it (finding 10), then either retitle its header and README.md:62 to "abbreviated illustrative example (GSTIC phases only)" or complete it against every exit criterion. Add to CLAUDE.md Orchestration Behavior: "Before producing the first Goals deliverable, read examples/warm-leads-gstic.md for the concreteness expected in KPI targets, baselines, owners and dates; the template governs structure." Long term, build a fictional full-fidelity set examples/<fictional>/0a through 5 plus a compiled brief.

**10. The example publishes a real company's internal initiative and a named employee in a public MIT repo** (high, M; F95, F116)
- Evidence: warm-leads-gstic.md:41 "First American's Owners Portal captures rich behavioral signals from homeowners"; :76 "Owner: JR De Los Reyes"; :109 "FAST file + Agent OID"; :118 "Dscope/Identity team"; :172 "First American owns the OP behavioral data AND the title/agent identity graph"; :286-287 a scoring formula and threshold; :330-372 FY2026 roadmap dates. The remote is the public jerimiahmertz/magstic-framework; the same text is in the zip, in both commits, and in the sibling public repo gstic-framework.
- Impact: Disclosure of an unannounced lead-generation program, its systems, targets, dates and model, plus a third party's name and role, with no evident consent. Reputationally sensitive for the company and the author.
- Fix: Rewrite with a fictional company (check the name has no real title-company match), generic system names ("Homeowner Portal", "Agent Mobile App", "identity service"), roles instead of people, and altered targets and dates. Purge history: `git filter-repo --path magstic-framework/examples/warm-leads-gstic.md --path magstic-framework.zip --invert-paths`, re-add the fictional file, force-push all branches, open a GitHub Support ticket for cached views. Apply the same rewrite to jerimiahmertz/gstic-framework or the purge achieves nothing. Alternatively obtain written sign-off from the company and the named person.

**11. Example numbers do not trace: two of three Goal KPIs vanish in Control, and Strategy names two different targets** (medium, M; G12, G13)
- Evidence: warm-leads-gstic.md:88 "KPI: Lead-to-close conversion rate" and :109 "% of OP users matched to FAST file + Agent OID" appear nowhere in Phase 5 (397-412); the North Star at :397 counts "opened title orders", a different funnel stage, with no target. :162 "Target Segment: Homeowners in Owners Portal" but :167 "Value Proposition: For Direct Division agents". analyst.md:463 requires "Every KPI from Phase 1 (Goals) must appear in the measurement framework".
- Impact: The artefact meant to show traceability breaks the Goals-to-Control chain README.md:31 advertises, and the positioning statement's target slot names a party that is not the declared segment.
- Fix: Add a three-row Goal KPI Coverage Check to the example's Phase 5, label the North Star as the leading proxy for the close rate and give it a target, restructure Phase 2 into two named segments (agents as the pipeline's customer, homeowners as the lead population with a guardrail), and add a Strategic Link column to the tactics table.

**12. Example owners, acronyms and its one external dependency are undefined** (low, S; G17, G19, G24)
- Evidence: :118 "Owner: Dscope/Identity team" while tactician.md:424 says "person, not team"; the Tactics (200-206) and Implementation (310-316) tables have no Owner column; nine terms are never defined (OP, FAST, Agent OID, Azure OID, Dscope, FA Mobile, Direct Division, TP, Amplitude); :359 "Segment integration (TP adopts Q2)" is the only external dependency and appears in no dependency or risk list.
- Impact: Readers cannot tell whether leads route to the agent of record or a pooled list, and Claude learns that team-level or absent ownership is acceptable.
- Fix: While anonymising, add Owner columns, a Glossary in the gstic-brief.md:98 format, and a two-row Dependencies list under Phase 4 with a fallback.

### Consistency across files

**13. README and CLAUDE.md advertise frameworks that no agent or template implements** (medium, S; F17, F25, F49, F104)
- Evidence: README.md:123 "OKRs, Balanced Scorecard", :124 "AI Canvas", :125 "Growth Loops", :127 "HEART Framework"; CLAUDE.md:117 lists HEART. `grep -rli` over agents/ and templates/ returns nothing for any of them. AARRR is claimed for Control (README.md:127) but lives only in business-model-canvas.md:80. MoSCoW (tactician.md:225) and Porter's generic strategies (value-positioning.md:111) are used but never claimed. Chernev's mix is implemented and should stay.
- Impact: Users expecting OKR goals or HEART metrics get none; Claude may bolt a HEART table onto a Control deliverable the template does not account for. The two READMEs and CLAUDE.md give three answers to "what frameworks does this use".
- Fix: Rewrite README.md:122-127 to what exists (objective and KPI worksheet; VPC, JTBD, positioning statement, 7 Powers; Chernev mix plus RICE/ICE/MoSCoW; P0-P3, RACI, risk register; North Star plus input, health and counter metrics, experimentation). Remove HEART from CLAUDE.md:117 and make that line the canonical per-phase list the README mirrors. Or implement each after restoring templates: KR rows in goals-worksheet.md, a HEART row group in control-dashboard.md, a Growth Loops subsection under Distribution.

**14. The framework is MAGSTIC only in the root README; every runtime message says GSTIC and the acronym is never expanded** (medium, S; F26, F100, F114)
- Evidence: README.md:1 "# MAGSTIC Strategic Planning Framework" and :5 "## What is MAGSTIC?" with no expansion. CLAUDE.md:1 "# GSTIC Framework", kickoff.md:28 "Welcome to the GSTIC Strategic Planning Framework", gstic-brief.md:107 "Document generated via GSTIC Framework", control-dashboard.md:1374 "GSTIC Framework Complete". README.md:113 uses GSTIC for all ten phases while README.md:19 uses it for Stage 2 only.
- Impact: A user clones MAGSTIC, is greeted by GSTIC, and gets a brief stamped GSTIC. "Run GSTIC" (Phases 1-5) and "Run full GSTIC" (all ten) read as contradictory commands.
- Fix: Add under README.md:3: "MAGSTIC = Market Attractiveness (Stage 1, Phases 0A-0E) + GSTIC (Goals, Strategy, Tactics, Implementation, Control). Below, GSTIC refers to Stage 2 only." Rename to MAGSTIC at CLAUDE.md:1 and :7, kickoff.md:1 and :28, phase-transitions.md:1, gstic-brief.md:1 and :107, control-dashboard.md:1374 (after restore), README.md:113. Keep kickoff.md:100 as the GSTIC-track banner. Re-run the grep after restoring from gstic-framework, which reintroduces v1 strings.

**15. Command tables exist in five places with drifting definitions** (low, S; F89, F107)
- Evidence: CLAUDE.md:136-145, kickoff.md:9-15, kickoff.md:171-180, README.md:43-48 and :100-109. "Run GSTIC" is "Phase 1 through 5 only" (CLAUDE.md:140) but "Full five-phase GSTIC run" (kickoff.md:175). "Show status" has three definitions. "Run Builder PM Stack for [project]" exists only at kickoff.md:13. README.md:105 "Run the Strategy phase for [project]" versus CLAUDE.md:141 "Run [phase] for [project]".
- Impact: Every edit must be made five times; "Run MAGSTIC" matches nothing.
- Fix: Keep one table in CLAUDE.md; delete kickoff.md:167-180 and :9-15; keep only the four-row Quick Start table in the README. Add a first row treating "Run MAGSTIC", "Run full pipeline" and "Run Builder PM Stack + GSTIC" as synonyms. Merge the two Step 0 banners (kickoff.md:28-38 and :100-107) with "(omit question 5 in GSTIC-only mode)".

**16. Tactician requires Owner and Dependencies columns the Tactics template does not have, and the one scoring table is RICE-only** (medium, S; F12, F23)
- Evidence: tactician.md:219 "Tactical Plan Table: One row per tactic, columns for Dimension, Tactic, Rationale, Owner (TBD ok), Dependencies". tactics-plan.md tables (78-90 and four siblings) use "Tactic / Description / Rationale / Strategic Link / Priority"; `grep -n Owner templates/tactics-plan.md` is empty. tactics-plan.md:563 offers "RICE / ICE / MoSCoW" but the only table (574-592) has RICE columns.
- Impact: Tactics ship with no owner, so Implementation (tactician.md:424 "Every deliverable must have an owner") starts with nothing to inherit; an ICE or MoSCoW choice has no table.
- Fix: In the restored tactics-plan.md change the five header pairs (lines 12-13, 18-19, 24-25, 30-31, 36-37) to "Tactic | Description | Rationale | Strategic Link | Owner (TBD ok) | Dependencies | Target Phase (P0-P3)", keep the Dependencies table (80-85) for blocked-by detail, and add a second MoSCoW table "Tactic | Must / Should / Could / Won't | Rationale".

**17. Strategy: agent and exit criteria want a value proposition per segment and a positioning statement; the template has one "Value Proposition Statement" in positioning form, and the statement is templated three ways** (medium, M; F24, F30)
- Evidence: strategist.md:286 "Value Proposition per segment", :289 "Positioning Statement"; phase-transitions.md:154-155 require both. strategy-canvas.md:353 has a single "Value Proposition Statement"; `grep -ni 'positioning statement' templates/strategy-canvas.md` is empty. Slot orders differ across customer-discovery.md:125-130, strategist.md:255 and strategy-canvas.md:364-377. phase-transitions.md:79 carries "Draft positioning statement (to be refined)" into 0D, which has no positioning section.
- Impact: The Phase 2 exit box "Positioning statement drafted" has no section to point at; the 0C draft is never finalised; the statement changes shape between phases.
- Fix: In the restored strategy-canvas.md rename line 43 to "### Positioning Statement" and add a "### Value Proposition (per segment)" table above it ("Segment | Key benefits | Costs / trade-offs | Why better than best alternative"). Adopt one six-slot form (Moore's order) in customer-discovery.md:125-130, add "## 5b. Positioning Statement (Final)" to value-positioning.md after line 134 with a matching 0D exit line, and point restored strategist.md line 57 at the canonical form.

**18. "Strategy Canvas" names two artefacts, and template file names sort out of phase order** (low, M; F31, F115)
- Evidence: value-positioning.md:39 "## 2. Strategy Canvas (Blue Ocean)" is a value-curve chart; templates/strategy-canvas.md:4 uses the same name for the whole Phase 2 deliverable. kickoff.md:74 and :122 use the phrase for both. `ls templates/` lists business-model-canvas (0E) first and market-attractiveness (0A) seventh. H1 style differs: "# Phase 0B: Opportunity Sizing" versus name-first titles in the GSTIC templates.
- Impact: "Produce the Strategy Canvas" in 0D can resolve to the Phase 2 file, which has no value curve, so the 0D exit criterion (phase-transitions.md:86) cannot be met with it.
- Fix: After restoring, `git mv` templates to phase-prefixed names (00a-market-attractiveness.md through 05-control-dashboard.md and 99-compiled-brief.md, with strategy-canvas.md becoming 02-strategy-plan.md), normalise every H1 to "# Phase N: Name", and update CLAUDE.md:76-94, kickoff.md:45-160, README.md:78-88 and gstic-brief.md:45. Rename value-positioning.md:39 to "Blue Ocean Strategy Canvas".

**19. "P0" means top priority in Tactics and the Discovery phase in Implementation, and the P0-P3 names differ across four files** (medium, S; F32, F44, F60, F140)
- Evidence: tactics-plan.md:113 "P0/P1/P2" (also 195, 277, 359, 441); implementation-roadmap.md:61 "P0: Discovery & Validation" through :790 "P3: Optimize & Expand"; tactician.md:397-406 "P0 (Discovery/Validation) ... P3 (Optimize)"; the example uses both meanings (:236 as priority, :327 as phase). The Tactics template also has a RICE "Rank", a third priority vocabulary.
- Impact: A "P2" tactic could mean third priority or lands in Scale; "P0 tactics ship in P1" becomes unreadable in the compiled brief.
- Fix: In the restored tactics-plan.md rename the column to "Target Phase (P0-P3)" with values "P0/P1/P2/P3" and let the Prioritization Rank be the only priority. Use the implementation-roadmap.md strings verbatim in restored tactician.md 77-80 and add one sentence under Phasing Principles that P0-P3 always mean roadmap phases. Rename the example's header to "Target Phase".

**20. Phase 2 re-collects segments, JTBD and competitors already produced in 0A/0C/0D; no template carries provenance, inputs or a per-phase filename** (medium, M; F33, F127, F141)
- Evidence: strategy-canvas.md:37 "Linked Goals" is the only link. Its Target Segments (50-240), JTBD (264-342) and Alternative Solutions (462-540) duplicate customer-discovery.md sections 1-3, market-attractiveness.md section 5 and value-positioning.md section 2. `grep -n 'Phase 0\|Market Discovery' templates/strategy-canvas.md agents/strategist.md` returns nothing. Phase 0 headers carry the fixed text "## Analyst: Market Analyst Agent" (market-attractiveness.md:5) and no Inputs field. Competitors are free text in four layouts.
- Impact: In a full run the Strategist re-asks "Who are the primary and secondary target segments?" (strategist.md:215) or silently copies; conclusions can be overwritten without a note, and a reader cannot tell which Goals version fed a Strategy deliverable.
- Fix: In every template header add "**Version:** v1", an "**Inputs:** | Phase | File | Date | Version |" table and a one-line change log used on `Revise`. Under restored strategy-canvas.md line 5 add "## Linked Market Discovery: [0E file + date, or not conducted]", and under Target Segments (9), JTBD (35), Positioning Statement (43) and Competitive Landscape (48) add "*If Phase 0 was completed, import from 0C sections 1-3 / value-positioning.md section 2 / market-attractiveness.md section 5 and record only what changed and why.*" Give competitors stable IDs (C1, C2, ALT-1) in market-attractiveness.md:94-98 and use them downstream. Insert a question 0 in strategist.md Phase 2: confirm which Market Discovery conclusions still hold.

**21. Handoff and completion scripts exist two or three times with drift and no stated order** (low, S; F35, F88)
- Evidence: the 0E to Goals handoff appears at kickoff.md:85, phase-transitions.md:117 and market-analyst.md:221 with different placeholders. Completion at kickoff.md:153 differs from phase-transitions.md:222. kickoff.md:89 points at the agent file for one handoff and at nothing for the other nine.
- Impact: Claude does not know whether to post the iteration question, the handoff, or both, and in which order.
- Fix: Make phase-transitions.md the single owner. Replace each kickoff "Ask:" line (48, 57, 66, 75, 85, 117, 126, 135, 144, 153) with "Run the A → B transition from prompts/phase-transitions.md", move the iteration prompts into the transition blocks, replace market-analyst.md:215-221 with a one-line pointer, and add under General Principles: "(1) print Exit Criteria with ticks; (2) ask the iteration prompt; (3) end the turn; (4) on confirmation post the Handoff Message; (5) record the phase Complete in STATUS.md."

**22. The Market Discovery Brief Summary is maintained in two templates and has already drifted** (low, S; F36, F142)
- Evidence: business-model-canvas.md:167 "[1-2 sentence verdict from Phase 0A]" versus gstic-brief.md:16, the same slot with "PESTEL assessment, Five Forces structural attractiveness, Wardley Map strategic plays"; :179 versus :28 on unit economics; :182 versus :31.
- Impact: Compiling the brief invites Claude to rewrite the Phase 0 summary rather than paste the approved one.
- Fix: Copy the richer wording into business-model-canvas.md lines 167, 173, 176, 179 and 182, add "Attractiveness score: [X]/5.0", and replace gstic-brief.md:15-33 with one line: paste section 5 of output/<slug>/0e-business-model.md verbatim. Add "[ ] Attractiveness score carried into the summary" to the 0E exit criteria.

**23. The Analyst requires Target, Baseline, Cadence and Action Trigger per metric; the Control tables omit them and the exit criteria never mention triggers** (low, M; F37, F72)
- Evidence: analyst.md:152-249 "For each metric: Name, Definition, Source, Owner, Target, Baseline, Cadence, Action Trigger"; analyst.md:466 "Every metric must have an action trigger". control-dashboard.md Input table (187-205) has no Action Trigger; Health (312-327) and Counter (403-418) have no Baseline, Target or Cadence. Action Triggers is a separate three-row table (956-1034). phase-transitions.md:211-218 omit triggers and the Goal KPI Coverage Check.
- Impact: Most metrics ship without a trigger, the "dashboard nobody looks at" the persona distrusts (analyst.md:27).
- Fix: After restore, add "Action Trigger (threshold → response)" to North Star and Input; add "Response", "Baseline" and "Cadence" to Health and Counter; retitle the Action Triggers section "Escalation Paths (derived)". Add two Control exit lines: every metric has a trigger; the Goal KPI Coverage Check shows every Phase 1 KPI covered.

**24. Quality checks that demand a stated tradeoff have no field in any template** (low, S; F39)
- Evidence: strategist.md:319 "Positioning must articulate a real tradeoff" and "what you're choosing NOT to be"; market-analyst.md:171 "Playing to Win must articulate a real tradeoff in Where to Play"; market-analyst.md:60 "If recommending No-Go, state what would need to change". strategy-canvas.md, value-positioning.md:103-108 and market-attractiveness.md:314-325 have no matching slot.
- Impact: The checks that separate a differentiated strategy from "better at everything" are unenforceable.
- Fix: Add "**What we are choosing NOT to be:**" after the positioning statement, "**Where we will NOT play:**" after value-positioning.md:108, and "**If Pivot Scope / No-Go, what would need to change to reconsider:**" after market-attractiveness.md:324, with matching exit-criteria lines under 0A, 0D and Strategy.

**25. 0B exit criteria ask for confidence ratings and SAM exclusion criteria the template lacks** (low, S; F28)
- Evidence: phase-transitions.md:47 "Assumptions logged with confidence ratings"; opportunity-sizing.md:108 has "Type (Validated / Estimated / Speculative)" but no Confidence column, unlike market-attractiveness.md:306. phase-transitions.md:44 "SAM defined with explicit inclusion/exclusion criteria"; `grep -ni exclusion templates/opportunity-sizing.md` is empty.
- Impact: Two 0B boxes cannot be ticked, and the 0A and 0B assumption logs cannot be merged into 0E.
- Fix: Use the unified assumptions schema (finding 57) in both files, and add "**Exclusion Criteria:**" after opportunity-sizing.md:45 plus an "Exclusions removed: $___" line in the SAM fence.

**26. The Full Pipeline sequence stops at the 0 to 1 transition and never says to continue with Run GSTIC Steps 1-5 without repeating Step 0** (low, S; F34)
- Evidence: kickoff.md:87-91 ends with "Deliver the handoff message from `agents/market-analyst.md`", then "## When the User Says Run GSTIC" (93) whose Step 0 (97-107) re-issues a welcome banner and re-asks project name, description and horizon.
- Impact: Read literally, the pipeline ends after Phase 0 or restarts with questions the user just answered.
- Fix: Replace kickoff.md:89 with "Deliver the 0E → Goals handoff from prompts/phase-transitions.md, then continue with Step 1 through Step 5 of the Run GSTIC sequence, skipping its Step 0."

### Methodology

**27. Tactics use five of Chernev's seven Ts; Service and Brand are missing** (medium, M; F47)
- Evidence: tactician.md:109 lists Product, Price, Incentives, Communication, Distribution; tactics-plan.md sections 61-389 match; phase-transitions.md:173 "all 5 dimensions". `grep -rn 'Service\|Brand' agents/tactician.md templates/tactics-plan.md` is empty.
- Impact: No run plans onboarding, support, SLAs or brand expression, and the exit criterion locks the omission in while README.md:125 labels the mix Chernev.
- Fix: In the restored tactician.md insert Service and Brand rows after line 31; add "### Service" and "### Brand" sections to tactics-plan.md after line 15; change phase-transitions.md:173 to seven dimensions. If grouping, use Chernev's own groups: designing value (Product, Service, Brand, Price, Incentives), communicating value, delivering value.

**28. Strategy defines the market by customers and competitors only; collaborators, company value and context are absent** (medium, M; F48)
- Evidence: strategy-canvas.md sections 50, 253, 383, 553, 684; `grep -rn -i collaborator .` returns nothing repo-wide; strategist.md:230 asks "Are there platform/ecosystem dynamics at play?" with nowhere to record the answer.
- Impact: A GSTIC-only run yields no channel or partner analysis, no statement of why the initiative creates value for the company, and no context scan, three of the five Cs a Chernev reader expects.
- Fix: In the restored strategy-canvas.md, between the Secondary Segment table (29) and Value Proposition (33), add Collaborators (Partner | Role | What they need | Value we offer), Company (capabilities, fit, resource commitment, expected profit) and Context (top three factors from market-attractiveness.md:70-73, or fresh). Add a "Customer value | Collaborator value | Company value" table above the positioning statement. Add matching Output Requirements to strategist.md (61-66) and three exit boxes.

**29. Goals never force one ultimate focus, a leading and lagging split, or a failure condition; the template hard-codes three objectives with one KPI each** (medium, S; F38, F51)
- Evidence: strategist.md:121 "2-4 primary objectives" with "1-2 KPIs"; :72 "Push for leading indicators alongside lagging ones"; :103 "What does failure look like?"; :170 asks for a proxy metric. goals-worksheet.md has exactly Objective 1-3 (91, 186, 281) with one KPI row each (122, 217, 312) and no leading, lagging, failure or proxy field. phase-transitions.md:133-138 never require the Goal Hierarchy, Assumptions or Open Questions the agent mandates.
- Impact: Chernev's Goal has a focus and benchmarks; the worksheet captures benchmarks only, so objectives sit side by side with no primacy. The proxy and anti-goal answers are lost.
- Fix: After restore, add a "Goal Focus" block (ultimate metric, type, benchmark), replace each KPI row with a per-objective table holding Lagging KPI, Leading indicator, Baseline, Target, Deadline, Measurement, Proxy and Owner (up to two rows), note "Use 2-4 objective blocks", and add a "Failure Condition / Kill Criteria" section. Add exit lines for ultimate goal, failure condition, hierarchy, assumptions and open questions; make the Control coverage check one row per KPI.

**30. Segment targeting is inverted: only the already-chosen primary target is scored, on criteria that disagree with the agent** (medium, S; F29, F56)
- Evidence: customer-discovery.md:17 segment table has no score; :28 "**Primary Target:**" precedes the 1-5 rubric at 32-39, which scores one segment on six criteria. market-analyst.md:119 names four criteria; phase-transitions.md:64 "3-4 segments identified and scored".
- Impact: The beachhead is asserted, not derived, so "why THIS segment first" (market-analyst.md:129) cannot be evidenced from the deliverable.
- Fix: Under section 1, after line 22, add "### Segment Scoring" with "| Criterion | Weight | Segment A | B | C | D | Evidence |", six criterion rows and a weighted total; delete 32-39; change line 28 to "highest weighted total, or state why you override"; trim WTP and Accessibility from the line-17 table; update market-analyst.md:119 to the six criteria.

**31. The Job Map is a purchase journey, not Ulwick's job map** (low, S; F53)
- Evidence: customer-discovery.md:66-73 "Define the need → Search for solutions → Evaluate options → Acquire/access the solution → Use → Monitor → Modify → Complete"; market-analyst.md:114 promises a "full job map"; `grep -rn Ulwick .` is empty.
- Impact: Steps 2-4 are buying friction, so "Biggest pain points in current job map" (75) surfaces purchasing pain rather than unmet execution outcomes, mis-framing the OST input.
- Fix: Replace 64-73 with an attributed table (Define, Locate, Prepare, Confirm, Execute, Monitor, Modify, Conclude) with Desired outcome, Importance, Satisfaction and Opportunity columns; keep the buying journey as an optional list; align the restored strategy-canvas.md JTBD scale (37-41) to 1-10.

**32. TAM/SAM/SOM: single-method choice, undefined Value-Theory, arbitrary SAM percentage, one capture rate for three years, ambiguous plus or minus 20%** (low, M; F55)
- Evidence: opportunity-sizing.md:20 "[ ] Top-Down | [ ] Bottom-Up | [ ] Value-Theory"; 48-52 "TAM × Serviceable percentage = SAM" despite four inclusion criteria at 41-45; :60 one "Market Share Assumption" against Y1/Y3/Y5 at 73-75; 86-89 rows Y1, Y2, Y3, Y5; 95-102 "±20%" without saying relative or absolute; `grep -rn -i triangulat .` is empty.
- Impact: Investor-grade sizing (market-analyst.md:7) triangulates; here Claude picks one method, fabricates the SOM ramp, and cannot rank which assumption matters most.
- Fix: Require at least two methods with a reconciliation line (update market-analyst.md:81 and README.md:117); derive SAM by filter; give SOM Y1/Y3/Y5 capture rates with SOM_Yn = SAM_Yn × capture_Yn; add Y4; define Value-Theory as "value created per customer × customers × share captured" or drop it; add "SOM Y1 at -20% | +20% | Rank by impact" columns and state "relative ±20%".

**33. Playing to Win "How to Win" offers Focus/Niche, which is a Where-to-Play choice** (medium, S; F57)
- Evidence: value-positioning.md:111 "[ ] Cost Leadership | [ ] Differentiation | [ ] Focus/Niche" under "## 4. Playing to Win Choice Cascade" (98); `grep -rn 'Lafley\|Martin' .` is empty.
- Impact: Porter's generic strategies are pasted into Lafley and Martin's cascade; "we win by being niche" dodges the choice that market-analyst.md:171 exists to force.
- Fix: Change line 111 to "**Basis of advantage:** [ ] Low cost | [ ] Differentiation, one or the other, applied within the Where-to-Play choices above"; retitle line 98 with the attribution; leave Porter's strategies in market-attractiveness.md:94.

**34. "LTV:CAC > 3:1 for viable SaaS" is stated as a consistency rule; LTV has no formula, margin, churn or payback anywhere** (medium, S; F59)
- Evidence: market-analyst.md:209; business-model-canvas.md:43-45 and :92 "CAC $____ / LTV $____ / LTV:CAC ___:1"; `grep -rn -i 'payback\|gross margin' .` is empty.
- Impact: A folk benchmark applied pre-launch to guesses; LTV on revenue rather than gross margin inflates it two to five times; non-SaaS models get the wrong yardstick.
- Fix: Rewrite line 209 to require the LTV formula, CAC basis, payback months and a model-appropriate benchmark. Extend business-model-canvas.md:92 with gross margin, churn, payback and the formula; require CAC and LTV as ranked rows in section 4 Riskiest Assumptions; update phase-transitions.md:109.

**35. The prioritisation table has no RICE formula, an undefined Score, and one column set for three methods** (low, S; F60)
- Evidence: tactics-plan.md:563-564 "Method Used: [RICE / ICE / MoSCoW]" followed by columns "Reach | Impact | Confidence | Effort | Score | Rank" (574-592); `grep -n -i formula templates/tactics-plan.md` is empty.
- Impact: Score is computed inconsistently or invented; a MoSCoW choice leaves the table unusable.
- Fix: Under Method Used add the formulas and scales (RICE = Reach × Impact × Confidence ÷ Effort with the standard scales; ICE = Impact × Confidence × Ease, each 1-10) and a second MoSCoW table.

**36. No kill criteria: go/no-go rows have no thresholds and every Control trigger says reassess** (low, S; F63)
- Evidence: control-dashboard.md:1235-1268 triggers end in "Reconvene", "Investigate", "Pause", "Reassess"; the only "Kill" is per experiment (1339); implementation-roadmap.md:153-157 "Go/No-Go Decision [What data informs the decision]" has no threshold; `grep -i failure templates/goals-worksheet.md` is empty.
- Impact: No pre-committed stopping rule, the main defence against sunk cost; "Kill bad ideas early" (market-analyst.md:26) stops applying after 0A.
- Fix: Add a Kill / Pivot Criteria table to goals-worksheet.md seeded from 0A Conditions for Proceeding and 0E experiments; make Go/No-Go rows "Go if ... by [date]; No-Go if ...; owner"; add a fifth Iteration Trigger and give "declining for 2+ weeks" a stated basis.

**37. Implementation has no budget, cost or financial case** (medium, M; F64)
- Evidence: implementation-roadmap.md:1146-1158 Resource Requirements are in hrs/wk only; `grep -n -i 'cost\|\$\|spend' templates/implementation-roadmap.md` is empty; tactician.md:94 asks "What's the budget envelope?" and :253 promises a cut line, with nowhere to record either.
- Impact: A plan with no cost by phase cannot be approved by the sponsor named in Goals, and the tactics cut line cannot be drawn.
- Fix: Add "## Budget by Phase" and "## Financial Case" (Y1 cost, expected impact from Goals KPIs, payback; source 0E Cost Structure) to implementation-roadmap.md; add "Budget envelope" and "Cut line" under tactics-plan.md Method Used; add an exit line "Budget by phase estimated and envelope stated (or explicitly N/A)".

**38. Control evaluates performance only; environment monitoring and cohort or retention views are absent** (medium, M; F65)
- Evidence: control-dashboard.md sections 50-1281; `grep -n -i 'competitor\|PESTEL\|market\|environment' templates/control-dashboard.md` is empty; retention appears nowhere in analyst.md or the template except as an experiment type.
- Impact: A competitor price cut or regulatory change never triggers a strategy revisit; the North Star can rise on acquisition while retention collapses, the failure analyst.md:141-147 claims to guard against.
- Fix: After restore, add "## Environment Monitoring" (Signal | Source | Cadence | Threshold | Triggers which phase) seeded from phase-transitions.md:204-207, and "### Cohort & Retention" with checkpoints; add analyst Output Requirements and two Control exit lines.

**39. Pricing is declared, never researched** (low, S; F66)
- Evidence: tactician.md:160 "What's the pricing model, tier structure, or cost to serve?"; `grep -rn -i 'conjoint\|westendorp\|willingness' agents templates` returns only column labels.
- Impact: Every downstream number depends on price, yet nothing asks how the price point was tested; "cost to serve" nudges cost-plus thinking.
- Fix: Under the restored tactics-plan.md "### Price" (17) add a Pricing Research block (method, price metric, reference price, value-based ceiling); reword restored tactician.md:32; move cost to serve to 0E Cost Structure or a Resource Requirements row.

**40. Moat is assessed in four unlinked places, and 7 Powers asks only Y/N** (low, S; F70)
- Evidence: strategy-canvas.md:570-576 "Power | Applicable? | Evidence / Plan"; market-analyst.md:147, value-positioning.md:113, business-model-canvas.md:70-71 and strategist.md:227 each ask again with no cross-reference.
- Impact: "Network Effects: Y" with no barrier stated; four moat answers can contradict across the brief.
- Fix: Put a 7 Powers table (Benefit | Barrier | Stage) in value-positioning.md section 4 replacing line 113; have business-model-canvas.md:70-71 and the restored strategy-canvas.md table (69-77) import it; reword strategist.md line 52.

**41. The Threat of New Entrants rating direction is never stated against barrier height** (low, S; F75)
- Evidence: market-attractiveness.md:101 "Rating: [ ] Strong | [ ] Moderate | [ ] Weak" followed by :103 "| Barrier to Entry | Height (H/M/L) |"; nothing says high barriers mean weak threat.
- Impact: "H" barriers plus "Strong" threat is a plausible error that propagates to scorecard row 289.
- Fix: Add under line 101 "High barriers → Weak threat; Low barriers → Strong threat" and relabel row 289 "Barriers to entry (high = 5)".

### Orchestration and prompting

**42. The Market Analyst is never told to search, fetch, read or ask for documents** (high, M; G01, G04, G06)
- Evidence: market-analyst.md:7 "evidence-driven", :56 "Every rating must have evidence or a stated assumption", :89 "no unsourced numbers". `grep -rniE 'web ?search|webfetch|research|cit(e|ation)|verif'` finds no instruction. kickoff.md:44-47 goes from "Ask the discovery questions" straight to "Produce the deliverable". market-analyst.md:75 "Are there existing industry reports?" and :109 "Do we have any direct customer evidence?" are never followed by "read them". kickoff.md:163 is the only filesystem path besides framework reads; no inputs/ directory exists.
- Impact: The roughly 221 empty cells and four Evidence columns of market-attractiveness.md are filled from recall and invention, then labelled Evidence. A user with twenty interview transcripts gets the same output as one with nothing. The framework runs in Claude Code and never uses its defining capability, reading local files.
- Fix: Add an "Evidence Acquisition" section to market-analyst.md and a "3b. Gather evidence" step to each Phase 0 block in kickoff.md: ask for documents, paths or URLs and Read or WebFetch each before output; WebSearch every named competitor, market figure and regulation; list sources at the end of the deliverable; if web tools are unavailable, say so in the header and mark external values unverified. Add an inputs/<project>/ convention (git-ignored except its README), a Step 0 question 6 "Do you have documents to ground this?" and a Step 0.5 that globs and reads them. Replace CLAUDE.md:130 with "read prior discovery before starting and treat it as the baseline to challenge".

**43. No provenance rule: user facts, recollection and invention are indistinguishable, and no citation format or fallback exists** (medium, M; G02, G05, G08, F62)
- Evidence: market-attractiveness.md:94 "| Competitor | Est. Market Share |" and :270-276 M&A, talent and funding signals with no source, date or unknown option; the PESTEL tables (23-67) have no Source column; opportunity-sizing.md:33-35 "**Sources / Basis:** - [Source 1]" with no publisher, year, URL or figure; :82 "Basis: [Industry report / historical trend / analogous market]" is a category pick. The only date field is "## Date:" at line 4.
- Impact: "$4.2B TAM" or "Acme: 18% share, weak enterprise support" appears with no way to tell who said it or when. "No unsourced numbers" with no fallback pushes Claude to invent a report title rather than admit an estimate.
- Fix: Define provenance tags in market-analyst.md Evidence Rules and CLAUDE.md Output Preferences: [U] user-stated, [S:n] sourced, [K] model knowledge unverified, [E] estimate with calculation shown, [?] unknown and left blank. Add a "Source / Grade" column (Validated / Estimated / Speculative, reusing opportunity-sizing.md:108) to the PESTEL, segment and VPC tables and "As of / Source" to competitor and dynamics tables. Replace opportunity-sizing.md:33-35 with a Sources table (Publisher | Title | Year | URL | Figure taken | How accessed) and add to market-analyst.md:89 "If no source exists, write Model estimate with reasoning; never cite a report you have not read this session." Write the model's knowledge-cutoff date in the header.

**44. A full pipeline asks 65 discovery questions in 11 rounds with no batching, dedup or thin-answer rule** (medium, M; F81)
- Evidence: kickoff.md:33-37 (5) plus market-analyst.md six per phase times five, strategist.md 88-103 and 215-230, tactician.md 88-103 and 301-316, analyst.md 86-101 equals 65. Repeats: kickoff.md:35 time horizon versus strategist.md:97; market-analyst.md:147 moat versus strategist.md:227; :106 alternatives versus strategist.md:218. No file contains "best judgment", "already answered" or "skip". "Wait for the user's response" appears only at kickoff.md:40 and :109.
- Impact: Users abandon multi-hour interrogations; the Strategist visibly re-asks four things 0C and 0D answered; with no rule for thin answers Claude stalls or invents; because "Ask" is followed directly by "Produce", Claude may ask and answer in one turn.
- Fix: Add to Orchestration Behavior: post all of a phase's questions in one numbered message and end the turn; delete questions already answered and open with a "From earlier:" recap; tell the user they may answer a subset or say "use your judgment"; record unanswered items as "ASSUMPTION (unvalidated)" in the Assumptions Log and never re-ask; at most four net-new questions per phase. Append "(skip if answered in Step 0)" to restored strategist.md lines 19 and 22 and prefix the Strategy questions with "If Phase 0 was completed, take segments, alternatives, switching forces and moat from 0C/0D." Add "Wait for the user's reply" after every "Ask" line in kickoff.md.

**45. "Never skip the designated entry point" contradicts the Skip to command in the same files** (low, S; F87)
- Evidence: phase-transitions.md:9 "Never skip the designated entry point"; CLAUDE.md:100 "Never skip Goals in a GSTIC run" versus CLAUDE.md:145 "`Skip to [phase]` | Jump ahead (with dependency warning)" and phase-transitions.md:230 "Want to proceed anyway".
- Impact: Claude either refuses a documented command or violates a bolded rule, differently per session.
- Fix: Pick one design and apply it end to end: keep Goals mandatory but allow skipping later phases after the warning and delete the sentence at phase-transitions.md:9; or make entry points defaults, record skipped phases in STATUS.md, and extend gstic-brief.md:92-96 with a Skipped value.

**46. "Run market discovery" and single-phase runs have no script, and the "market context seems thin" test is undefined** (medium, S; F21, F92)
- Evidence: kickoff.md:9-15 routes "Run market discovery" and "Run [specific phase]", but the file has only "Run Full Pipeline" (19) and "Run GSTIC" (93). The Full Pipeline banner (28-30) promises Phases 1-5 and 0E ends "Ready to move into GSTIC" (85). CLAUDE.md:47-49 promises a Market Discovery Brief Summary as the final deliverable. kickoff.md:109 "If market context seems thin" gives no criterion.
- Impact: A market-validation user gets a banner promising ten phases and is pushed into Goals; a single-phase user gets no context gathering; "thin" is applied inconsistently.
- Fix: Add "## When the User Says Run Market Discovery" (Step 0 with its own banner, Phases 0A-0E, then deliver business-model-canvas.md section 5 and ask "continue into Goals, or stop here?") and "## When the User Names a Single Phase" (Step 0 unless STATUS.md exists, then that phase's block). Replace :109 with a fifth GSTIC Step 0 question ("Has any market discovery been done? Name the target segment and the main alternative.") and a rule keyed to its answer. Point CLAUDE.md:47-49 at the new section.

### Documentation and onboarding

**47. Customization advice is wrong: new agents, templates and examples are not discovered automatically** (low, S; F103)
- Evidence: README.md:131-134 "Add new agent personas in `agents/`", "Add templates", "Add worked examples". CLAUDE.md:58-67 and :71-94 are fixed tables naming exact files; kickoff.md hard-codes each Read; `grep -rn examples CLAUDE.md prompts/` is empty.
- Impact: A team that drops growth-engineer.md into agents/ never sees it activated.
- Fix: Rewrite 131-134 to say each addition must be registered in CLAUDE.md's tables and kickoff.md's Read steps, and add a "## Calibration Examples" section to CLAUDE.md. Note that only .claude/agents/ subagents and `@` imports are auto-discovered.

**48. No "What to expect" section: time, question count, prerequisites and outputs are unstated** (low, M; F105)
- Evidence: README.md:33-50 goes from `claude` to the command table; `grep -n -i 'prerequisite\|hours\|expect' README.md` matches only two table cells. The files require 65 questions, ten review checkpoints, competitor names, budget, baselines and analytics tools.
- Impact: Users start unprepared, answer "I don't know" repeatedly, and abandon with nothing saved.
- Fix: Add "## What to expect" after Quick Start: prerequisites (Claude Code installed and authenticated), effort per mode (about 65, 35 and 34 questions), what to prepare, outputs and where they land, and how to resume with `claude --continue` then `Show status`.

### Repository hygiene

**49. The committed magstic-framework.zip is a byte-identical, stale, unreferenced copy** (medium, S; F109)
- Evidence: `unzip -l` shows 20 files; `diff -r` against magstic-framework/ differs only by .DS_Store; the zip's README is the nested "# GSTIC" copy while README.md:1 says MAGSTIC; no README mentions the zip.
- Impact: A binary duplicate that diverges on every edit and ships the stripped files with no way to see it.
- Fix: `git rm magstic-framework.zip`; add `*.zip` to .gitignore; point users to GitHub's Code, Download ZIP, or a Release.

**50. Two .DS_Store files, no .gitignore, output/ written into the repo tree, and a README tree that omits all of this** (low, S; F41, F102, F111, F112)
- Evidence: `git ls-files` includes `.DS_Store` and `magstic-framework/.DS_Store`; `find . -name .gitignore` is empty; kickoff.md:163 saves to output/, which does not exist; the README.md:67-96 tree omits the zip, .DS_Store, the extra directory level and output/.
- Impact: Finder noise in every clone; the first `git add -A` in a fork commits a confidential brief; the documented tree is not the real tree.
- Fix: .gitignore with `.DS_Store`, `._*`, `*.zip`, `output/*`, `!output/.gitkeep`; commit output/.gitkeep; regenerate the README tree with `git ls-files | tree --fromfile` after flattening and annotate output/; add an "Outputs" note after the Usage Modes table.

**51. Two near-identical READMEs that have already drifted** (medium, S; F99, F114)
- Evidence: `diff README.md magstic-framework/README.md` differs on exactly lines 1, 3, 5 and 7 (MAGSTIC versus GSTIC; "Market Discovery/Attractiveness" versus "Market Discovery"); the second commit edited only the root copy.
- Impact: GitHub shows MAGSTIC; the subdirectory and zip show GSTIC.
- Fix: `git rm magstic-framework/README.md` as part of the flatten; keep the root README as the single landing page.

**52. README claims MIT but no LICENSE file exists** (low, S; F101, F113)
- Evidence: README.md:136-138 "## License" then "MIT"; `find` for LICENSE*, COPYING* returns nothing at either level. The sibling gstic-framework has none either, so a copy will not bring one.
- Impact: GitHub detects no license; corporate OSS policies treat the repo as unlicensed.
- Fix: Add a root LICENSE with the full MIT text and "Copyright (c) 2026 Jerimiah Mertz" (confirm the holder); change README.md:138 to "MIT, see [LICENSE](LICENSE)".

**53. No tag, release, CHANGELOG or CONTRIBUTING despite a "v2" commit** (low, S; F106, F117)
- Evidence: `git log` shows f24efed "v2: MAGSTIC Framework ..." and 5d1c8d3 "Update README.md", both with empty bodies; `git tag -l` is empty; no version string in any .md; two author identities.
- Impact: Nothing to pin, no record of what changed from v1, and no guidance to stop the formatting loss recurring.
- Fix: After restore and flatten, `git tag -a v2.0.0`, create a Release, add CHANGELOG.md with 2.0.0 (Phase 0 added) and 1.0.0 (link to gstic-framework), and CONTRIBUTING.md (keep headings and tables, run markdownlint, register new files in CLAUDE.md, fictional names only in examples/).

**54. Trailing whitespace, language-less fences, no .editorconfig** (low, S; F120)
- Evidence: market-attractiveness.md:117 delimiter row ends with a space; 199-217 all carry trailing spaces; markdownlint reports 18 MD009, 14 MD040 and 4 MD036 across the intact files.
- Impact: Noisy diffs and fences without hints; editors will reintroduce it.
- Fix: Add .editorconfig (lf, final newline, trim trailing whitespace, utf-8), strip the spaces, label fences `text`, and add .markdownlint.json disabling MD013 and enabling MD041 so a stripped file with no H1 fails lint.

### Template design

**55. The compiled brief pastes five whole deliverables and condenses Phase 0 to twenty lines** (medium, M; F122)
- Evidence: gstic-brief.md:39, 45, 51, 57, 63 "[Paste completed ... content here]"; Phase 0 at 11-33 is a short summary; line 7 asks five questions in "3-5 sentences".
- Impact: Thousands of lines with no per-phase takeaway; the largest body of work gets three sentences per phase; pasted H1s collide with the brief's H2s.
- Fix: Replace each paste slot with "### Key decisions", "### Headline table" (per-phase columns), "### Open assumptions" and a link to output/<slug>/<phase>.md in a new Appendix E; paste the enriched 0E section 5 for Phase 0; rewrite line 7 as five labelled one-liners; update kickoff.md:161 and CLAUDE.md:144.

**56. Inline "[ ] A | [ ] B" selectors collide with task-list syntax and have no marking rule** (low, S; F123)
- Evidence: 40 "[ ]" in market-attractiveness.md; :82 "**Rating:** [ ] Strong | [ ] Moderate | [ ] Weak" versus :322-324 real to-dos; :328 "Completed: [ ] Yes / [ ] No".
- Impact: Renders as literal brackets; Claude guesses between "[x]", deleting options, or bolding.
- Fix: Reserve "- [ ]" for checklists; convert inline selectors to "[Strong / Moderate / Weak]" at the listed lines and in the restored GSTIC templates; add to CLAUDE.md Output Preferences: "a bracket with slash-separated options means write exactly one option".

**57. No cross-phase assumptions log; five schemas and four templates with none** (medium, M; F125)
- Evidence: market-attractiveness.md:306, opportunity-sizing.md:108, business-model-canvas.md:154, goals-worksheet.md:431-449 and strategy-canvas.md:701-730 all differ; customer-discovery.md, value-positioning.md, tactics-plan.md and control-dashboard.md have none; phase-transitions.md:125 "Goals should address validating these" has no link.
- Impact: An 0B assumption ("20% capture rate") cannot be referenced from Tactics or validated in Control.
- Fix: One schema in every template's final section: "| ID | Assumption | Type | Confidence | Impact if wrong | Validation method | Owner | Status |" with zero-padded phase IDs (A-0A-01); 0E may prepend Rank and Category; add "Validates assumption ID" to the Learning Loops and Experimentation tables; add "### E. Assumptions Register" to gstic-brief.md after line 103.

**58. market-attractiveness.md is 25 tables, 40 selectors and 3 diagrams with no lite path** (low, L; F128)
- Evidence: CLAUDE.md:116 "Tailor depth to the user's context", but only two sections in eleven templates are optional (control-dashboard.md:1281, gstic-brief.md:13); PESTEL alone is six tables with 23 rows.
- Impact: Claude emits a 300-line document of one-word cells or silently drops sections; the exit criteria treat everything as required.
- Fix: Tag sections Required or Optional, add "**Depth:** [Lite / Full]" to the header and a Step 0 question, mirror Lite and Full in phase-transitions.md:21-28 and market-analyst.md:44-60, and keep a minimal competitor list in Lite. Apply to value-positioning.md and the restored GSTIC templates.

**59. Section guidance is missing or placed after the fill target** (low, S; F129)
- Evidence: value-positioning.md:122 "Management Systems Required" has no purpose line; :56 "*Rate each 1 (low) to 5 (high)*" follows the table at 45-54; market-attractiveness.md:266 axis instruction follows the diagram at 250; the scorecard (284-294) never states Weighted Score = Score × Weight.
- Impact: In a streaming answer the instruction is read after the table is produced; "Management Systems" gets treated as IT inventory.
- Fix: One italic purpose line under each heading that lacks one; move :56 above 45 and :266 above 250; make the scorecard header explicit; attribute the job map to Ulwick and management systems to cascade step 5.

**60. Footers use three conventions and the GSTIC ones assert completion unconditionally** (low, S; F46, F130)
- Evidence: market-attractiveness.md:328 "*Completed: [ ] Yes / [ ] No*"; goals-worksheet.md:480 "Phase 1 Complete"; control-dashboard.md:1374 "GSTIC Framework Complete"; headers differ ("## Analyst:" versus "Time Horizon:"). implementation-roadmap.md Key Milestones (1390) appear in no agent requirement or exit criterion.
- Impact: A deliverable says Complete the moment it is instantiated, contradicting `Show status`; nobody records who confirmed or when.
- Fix: One "## Status" block (Draft / In review / Complete, Confirmed on, Next phase) on all ten templates, aligned with the gstic-brief.md Appendix C vocabulary; standardise headers on "# Phase N: Title" plus Project, Date, Agent, Linked; add a Key Milestones exit line and tactician output requirement.

**61. Wardley Map dependency edges exist only in a tree that contradicts its own placement rule** (medium, S; F131, G31)
- Evidence: market-attractiveness.md:198-218 nest components by depth; :220 "Position left-to-right by evolution stage" cannot be honoured by a tree; the components table (183-194) has no dependency column.
- Impact: Claude either loses the evolution axis or breaks the box drawing; a screen reader loses the hierarchy; the chain cannot be carried into the brief.
- Fix: Add "Depends On (#)" to the table at 183-184; mark the diagram optional and derived from the table, or render it as mermaid `graph TD` or an OnlineWardleyMaps text block.

**62. Value curve, positioning maps and the Phase 2 matrix are unfillable in ASCII; the intact matrix in gstic-framework is already a table** (medium, S; F77, F133, G30, G34)
- Evidence: value-positioning.md:59-69 is an empty grid whose legend at :68 uses the same "──" glyph for Competitor A, Competitor B and Our Offering; market-attractiveness.md:250-264 scatter places names by column offset; strategy-canvas.md:414-451 is nine orphan paragraphs with "[Dimension A] →" twice; the 1-5 "high" is ambiguous for Price.
- Impact: Three overlapping curves cannot be drawn in monospace, so the mandated divergence (line 71) is invisible; the 2×2 the Strategist is told to use (strategist.md:292) cannot be produced.
- Fix: Replace value-positioning.md:58-69 with a letter-grid table (A, B, I, * placed in the score column) and state "for Price, 5 = highest price level (Kim & Mauborgne)"; replace the scatter with axis lines and a "| Player | X | Y | Note |" table; copy strategy-canvas.md from gstic-framework, where the matrix (its lines 52-55) is already a table, and relabel the duplicate headers "Low [Dimension A]" and "High [Dimension A]". Update market-analyst.md:160 to "chart optional".

**63. Four Forces of Progress is a fixed-width box with no strength field** (low, S; F134)
- Evidence: customer-discovery.md:81-115 two 44-character frames with roughly 34-character bullet slots; :117 net verdict is a checkbox selector.
- Impact: Real observations overflow and Claude truncates to keep the box; the net assessment has nothing to be computed from.
- Fix: Replace with "| Force | Direction | Factor | Strength (H/M/L) | Evidence |" seeded with the four forces; make :117 "**Net Assessment:** [Driving stronger / Resisting stronger / Marginal], one sentence why"; keep the box as an illustration only.

**64. The Opportunity Solution Tree anchors on a Phase 1 Goal that does not exist yet, hard-codes 3×2×1, and its tables cannot be joined** (medium, S; F13, F40, F52, F135)
- Evidence: business-model-canvas.md:105-106 "DESIRED OUTCOME / [From Phase 1 Goal]" while 0E precedes Phase 1 (CLAUDE.md:38-40); the diagram (103-124) fixes three opportunities, two solutions each, one experiment each; "Solutions Mapped" (136-140) and "Experiments" (144-148) share no key; the Priority column (128-132) has no criteria.
- Impact: In every full or market-only run Claude invents a goal the Strategist then reconciles; readers cannot tell which experiment tests which solution.
- Fix: Change 106 to an outcome hypothesis (a measurable product outcome from section 2, framed by the 0C JTBD) with a re-link note; replace the fixed diagram with an optional note that shape varies; use two keyed tables (Opportunities with Torres's size, market, company and customer factors and a Target? column; Solutions and assumption tests keyed by Opp # and Sol # with a pointer to the section 4 rank); update market-analyst.md:202 and phase-transitions.md:110; add a reconciliation line under goals-worksheet.md Business Context after restore.

**65. The Lean Canvas grid is misaligned, 95 columns wide, misplaces Key Metrics and Channels, and the writeup omits Customer Segments** (medium, M; F14, F54, G27)
- Evidence: bar positions vary per line (line 12 at [0,18,36,54,72,90] versus lines 14-25 at [0,19,38,57,75,94]); lines 26-27 put KEY METRICS under PROBLEM and CHANNELS under UVP, leaving two cells empty; lines 13-35 are 94-95 display columns; writeup headings (53-94) have no Customer Segments or Early Adopters; market-analyst.md:199 mandates "visual + detailed writeup".
- Impact: In an 80-column terminal every line wraps and no row aligns; on GitHub the fifth column scrolls off; screen readers get five boxes interleaved; filled canvases inherit the misalignment and the missing box.
- Fix: Add "### Customer Segments" (target, early adopters) and a High-Level Concept line to the writeup and attribute the canvas to Maurya. Either drop the grid and make the writeup canonical, or redraw at 78 columns or less with one fixed geometry (Key Metrics under Solution, Channels under Unfair Advantage) and check it with a ten-line script that prints box-character positions; say above the fence "text version of every box follows in section 2".

**66. The VPC table misaligns three independent lists, and the ERRC grid splits its labels across rows** (low, S; F136)
- Evidence: value-positioning.md:13-22 row 5 "| **Emotional:** | **Most Critical Pain:** | **Most Desired Gain:** |"; the Value Map (28-30) references "Pain [#]" without stable IDs; ERRC (78-94) lines 80-81 break one sentence across two table rows.
- Impact: Claude fills row by row and pairs pain 2 with gain 2; ERRC renders instruction fragments as data.
- Fix: Three ID'd lists (J1, PN1, GN1), a Value Map table keyed by those IDs, and one ERRC table "| Action | Factor | Rationale |" with per-quadrant why lines and the "at least one Eliminate and one Create" note (market-analyst.md:170).

## Roadmap

**Quick wins (under an hour each)**
- Flatten the repo and the hygiene commit: F1/F18/F78/F94/F108, F109, F111, F112, F41/F102, F99/F114, F101/F113, F120.
- Copy the nine intact files from gstic-framework and verify counts: F9, F10/F86, F11 (copy step only).
- Add the `@` imports, the mandatory-Read rule and the exit-criteria gate to CLAUDE.md: F2/F20/F79/F98, F3/F19.
- One-line fixes: CLAUDE.md:37 trigger phrase (F107), kickoff.md:89 continuation (F34), delete the "never skip" contradiction (F87), Threat of New Entrants note (F75), Playing to Win basis of advantage (F57), 0B exclusion and confidence (F28), tradeoff fields (F39), selector convention (F123), section purpose lines (F129), footers (F46/F130), Job Map (F53), RICE formulas (F60), pricing block (F66), 7 Powers columns (F70), VPC and ERRC tables (F136), Four Forces table (F134), Customization text (F103).
- Naming pass and single command table: F26/F100/F114, F89.
- Tag v2.0.0, CHANGELOG, CONTRIBUTING, LICENSE: F106/F117, F101/F113.

**Structural fixes (a few hours each)**
- Persistence with output/<slug>/ and STATUS.md, plus the compact SessionStart hook: F4/F22/F83/F84/F97, F85.
- Evidence acquisition, inputs/ convention and provenance tags for Phase 0: G01/G04/G06, G02/G05/G08/F62.
- Question batching and dedup rules: F81. Market discovery and single-phase scripts: F21/F92. Single-source handoffs: F35/F88.
- Anonymise the example and rewrite history in both repos: F95/F116.
- Tactics template columns, Service and Brand, P0-P3 vocabulary: F12/F23, F47, F32/F44/F140.
- Strategy template: positioning statement, per-segment value proposition, 5 Cs, Phase 0 carry-forward and provenance headers: F24/F30, F48, F33/F127/F141.
- Goals focus, leading and lagging KPIs, kill criteria: F38/F51, F63. Control action triggers, environment monitoring, retention: F37/F72, F65. Budget and financial case: F64. LTV formula: F59. Sizing rigor: F55. Segment scoring: F29/F56.
- Unified assumptions log: F125. Brief redesign: F122. Market Discovery Brief single source: F36/F142.
- Diagrams: Lean Canvas (F14/F54/G27), OST (F13/F40/F52/F135), value curve and matrices (F77/F133/G30/G34), Wardley dependencies (F131/G31). Template renames (F31/F115). "What to expect" section (F105).

**Strategic upgrades (a day or more)**
- Skills-based structure with per-phase runbooks and slash commands: F6, F3 long-term option.
- A fictional, full-fidelity example set covering 0A through 5 plus a compiled brief, replacing the summary: F42/F96/F67, G12/G13, G17/G19/G24, F93.
- Lite and Full depth tiers across all templates, agents and exit criteria: F128.
- Implement (rather than remove) the claimed OKR, HEART and Growth Loop frameworks if you want them: F17/F25/F49/F104.

## What is already strong

- agents/market-analyst.md is a model persona: per-phase mandate, six discovery questions, frameworks, output requirements and quality checks (lines 28-60 for 0A), with sharp rules such as "Every rating must have evidence or a stated assumption" (:56) and "Kill bad ideas early" (:26).
- prompts/phase-transitions.md contains exactly the right operational artefacts: per-phase Exit Criteria checklists (21-28 through 211-218), Handoff Messages with placeholders (32, 117, 142), "Context to Carry Forward" lists (34-38), and out-of-sequence scripts (226-241). It only needs to be wired in.
- The five Phase 0 templates are intact and specific: the weighted attractiveness scorecard (market-attractiveness.md:284-294), an assumptions log with "What Changes If Wrong" (:306), a sensitivity table (opportunity-sizing.md:95-102), the ERRC grid and Playing to Win cascade (value-positioning.md:78-126), and a riskiest-assumptions table with evidence for and against (business-model-canvas.md:154).
- CLAUDE.md's Orchestration Behavior (98-106), Agent Activation Rules (58-67) and Template Mapping (71-94) are compact, readable lookup tables that a model can follow once they are loaded.
- prompts/kickoff.md uses a consistent per-phase shape, Read agent, Read template, Ask, Produce, Ask to iterate (42-89, 111-153), which is the right skeleton for an orchestration script.
- Traceability is designed into the artefacts: Goal KPI Coverage Check (control-dashboard.md:479), Traceability Matrix (tactics-plan.md:473), Phase Completion Tracker (gstic-brief.md:83-96), and "Linked Goals" and "Linked Strategy" header fields.
- The text of the nine stripped files is complete: every exit-criterion item in phase-transitions.md:133-218 has a matching section, and the sibling gstic-framework repo makes recovery a copy rather than a rewrite.
- The README's Quick Start and Usage Modes tables (README.md:36-48, 100-109) are short and clear; once the layout matches them, onboarding is three commands.
