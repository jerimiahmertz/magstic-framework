# GSTIC Example — Warm Leads Initiative

> This example demonstrates a completed GSTIC run for a real enterprise initiative. It shows the level of specificity and traceability expected at each phase.

---

## Phase 1: Goals

**Project:** Warm Leads — Direct Division Lead Generation from Owners Portal

**Problem Statement:** First American's Owners Portal captures rich behavioral signals from homeowners (logins, listing alerts, equity checks) but none of this intent data flows to Direct Division agents who could convert it into new business. Revenue is being left on the table.

**Time Horizon:** FY2026 (Q1 Discovery through Q4 Optimization)

### Objectives

**Objective 1: Generate qualified leads from behavioral signals**
- KPI: Number of warm leads generated per month
- Baseline: 0 (no current pipeline)
- Target: 500 leads/month by Q3 2026
- Owner: JR De Los Reyes

**Objective 2: Prove lead-to-conversion viability**
- KPI: Lead-to-close conversion rate
- Baseline: N/A (new pipeline)
- Target: 2% conversion rate by Q4 2026
- Owner: TBD (Direct Division)

**Objective 3: Establish identity bridge between systems**
- KPI: % of OP users matched to FAST file + Agent OID
- Baseline: Unknown — requires P0 audit
- Target: 60% match rate by Q2 2026
- Owner: Dscope/Identity team

### Non-Goals
- Replacing existing lead sources (referral, marketing)
- Cold outbound to non-OP users
- Building a full CRM — leads route through FA Mobile

---

## Phase 2: Strategy

**Target Segment:** Homeowners in Owners Portal who exhibit refinance, sale, or equity-extraction intent signals within 6-18 months of their last close.

**Value Proposition:** For Direct Division agents, who need a steady stream of qualified leads, Warm Leads is an intent-based pipeline that surfaces homeowners already in the First American ecosystem showing buying signals, unlike cold lists or aged referrals, which lack behavioral context.

**Competitive Advantage:** Cornered resource — First American owns the OP behavioral data AND the title/agent identity graph. No competitor has both.

**Key Assumption:** Behavioral signals (logins, equity checks, listing alert frequency) are meaningfully correlated with near-term transaction intent. Must validate in P0.

---

## Phase 3: Tactics (Summary)

| Dimension | Tactic | Priority |
|-----------|--------|----------|
| Product | Intent scoring model (Lifecycle + Behavioral) | P1 |
| Product | Identity bridge (Azure OID → FAST → Agent OID) | P0 |
| Distribution | Lead routing through FA Mobile | P1 |
| Communication | Agent enablement — how to work a warm lead | P1 |
| Incentives | Pilot with top 20 Direct agents, share early wins | P1 |

**Scoring Formula:** Intent Score = Lifecycle Score (years since close, 0-5) + Behavioral Score (logins, alerts, equity checks, article views, 0-5). Threshold ≥7 triggers lead.

---

## Phase 4: Implementation (Summary)

| Phase | Timeline | Objective |
|-------|----------|-----------|
| P0 Discovery | Now (Q1 2026) | Validate identity bridge feasibility, audit signal availability |
| P1 Prototype | Q1-Q2 2026 | Amplitude-based scoring, manual lead routing to pilot agents |
| P2 Automated | Q2-Q3 2026 | Segment integration (TP adopts Q2), automated pipeline |
| P3 Optimize | Q4 2026+ | ML-refined scoring, expanded agent pool, feedback loops |

---

## Phase 5: Control (Summary)

**North Star:** Monthly Warm Leads that convert to opened title orders

**Input Metrics:** OP active users with intent score ≥7, lead delivery rate, agent response time

**Health Metrics:** False positive rate (leads with no intent), agent satisfaction with lead quality

**Counter-Metrics:** OP user churn (are we creeping people out?), agent time-on-lead vs. other sources

**Review Cadence:** Weekly (PM + Eng), Monthly (Direct Division leadership), Quarterly (Executive)

---

*This example is illustrative. Actual GSTIC outputs should be fully populated using the templates.*
