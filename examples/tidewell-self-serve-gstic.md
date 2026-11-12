# GSTIC Strategic Brief — Tidewell Self-Serve Trial

> **Fictional example.** Tidewell, its numbers, and its plans are invented to show a complete GSTIC run, compiled with `templates/gstic-brief.md`. Any resemblance to a real company is coincidental. Owners are role titles, not people.

## Executive Summary

Tidewell's Starter plan ($79 a month for shops with 1–4 technicians) is sold only through sales today, and onboarding costs about $1,200 per account — more than a year of Starter revenue — so sales turns most small shops away. This plan opens a self-serve channel in FY2027: owners sign up, set up, and pay without a sales call. Success means 45% of trials sending a first invoice within 7 days by September 2027, then 250 new paid self-serve accounts a month at a CAC payback of 6 months or less by December 2027. We target owner-operators of 1–4 technician shops and position against field-service suites on speed of setup. One North Star (weekly invoicing accounts) and stage gates in February, June, and September tell us whether it's working.

---

## 1. Goals

**Time Horizon:** FY2027 (January–December 2027)

### Business Context

**Problem/Opportunity Statement:**
Tidewell sells scheduling and invoicing software to home-services businesses (plumbing, HVAC, electrical). Its Starter plan ($79/month, 1–4 technicians) can only be bought through sales — a demo plus two setup calls that cost about $1,200 per account. Sales therefore focuses on shops with 5+ technicians and turns away most of the ~900 small-shop inquiries it gets each month.

**Executive Sponsor:** Chief Revenue Officer

**Current State Baseline:**
Sales closes about 60 new accounts a month, about 15 of them 1–4 technician shops. Payback on those small sales-led accounts is about 15 months. 31% of new accounts send a first invoice within 7 days of signing. There is no self-serve signup, trial, or card checkout.

### Primary Objectives

#### Objective 1: Open a self-serve channel for small shops
| Field | Value |
|-------|-------|
| KPI | New paid self-serve accounts per month |
| Baseline | 0 |
| Target | 250/month |
| Deadline | Dec 31, 2027 |
| Measurement Method | Billing + CRM: trials that converted to paid with no sales touch |
| Owner | Head of Growth |

#### Objective 2: Get trial users to value fast
| Field | Value |
|-------|-------|
| KPI | Activation rate: % of trials that send a first invoice within 7 days of signup |
| Baseline | 31% (proxy: sales-onboarded accounts, 7 days after signing) |
| Target | 45% |
| Deadline | Sep 30, 2027 |
| Measurement Method | Product analytics, `invoice_sent` event |
| Owner | Product Lead, Onboarding |

#### Objective 3: Keep the channel profitable
| Field | Value |
|-------|-------|
| KPI | CAC payback for self-serve accounts (months) |
| Baseline | ~15 months (sales-led, 1–4 technicians) |
| Target | ≤ 6 months |
| Deadline | Dec 31, 2027 |
| Measurement Method | Finance model: (acquisition + onboarding cost) ÷ monthly revenue per new account, by monthly cohort |
| Owner | Finance Business Partner, Growth |

### Goal Hierarchy

```
Make 1–4 technician shops a profitable growth segment in FY2027
├── Objective 1: Open a self-serve channel for small shops
│   └── New paid self-serve accounts: 250/month by Dec 2027
├── Objective 2: Get trial users to value fast
│   └── Activation within 7 days: 45% by Sep 2027
└── Objective 3: Keep the channel profitable
    └── CAC payback: ≤ 6 months by Dec 2027
```

### Non-Goals (Explicit Exclusions)

1. Moving shops with 5+ technicians off sales-led onboarding — they stay sales-led
2. Changing Starter pricing or packaging — this initiative changes how shops buy, not what they buy
3. New product features beyond onboarding and import, or new trades and countries

### Assumptions

1. Owners of 1–4 technician shops will finish setup without a human if setup covers customer import and the first invoice
2. The billing system can take card payments for self-serve signups by May 2027
3. Self-serve won't pull shops with 5+ technicians away from sales-led plans

### Open Questions

1. 14-day or 30-day trial? (Tested in Stage 2)
2. Does trial chat support count toward CAC? (Finance to confirm in January)

---

## 2. Strategy

**Linked Goals:** Objectives 1–3

### Target Segments

The primary segment drives every Stage 0–2 decision. The secondary segment is served through a handoff to sales, not a separate self-serve path.

#### Primary Segment: Owner-operators of 1–4 technician shops
| Field | Description |
|-------|-------------|
| Who | Owner who still works jobs; plumbing, HVAC, or electrical; US |
| Size | ~600,000 US shops (illustrative estimate) |
| Current Behavior | Paper job tickets, a spreadsheet of customers, invoices typed up at night or in a generic invoicing app |
| Pain Points | Evenings lost to paperwork; slow payment; double-booked jobs |
| Switching Trigger | A missed payment or a double-booking; hiring a first employee |
| Switching Cost | Re-entering the customer list; learning a new tool mid-season |

#### Secondary Segment: Office managers at 5–10 technician shops
| Field | Description |
|-------|-------------|
| Who | Office manager (often a family member) who runs dispatch and billing |
| Size | ~90,000 US shops (illustrative estimate) |
| Current Behavior | Shared calendar plus desktop accounting software |
| Pain Points | Dispatch by phone; chasing technicians for job notes; reconciling invoices with the books |
| Switching Trigger | Outgrowing the calendar; the accountant asks for cleaner books |
| Switching Cost | Migrating job history; retraining technicians |

### Value Proposition

#### Primary Segment: Owner-operators of 1–4 technician shops

| Job Type | Job Statement | Importance | Satisfaction (Current) |
|----------|--------------|------------|----------------------|
| Functional | Get a job scheduled, done, and paid without paperwork at night | High | Low |
| Emotional | Feel in control of the business instead of buried by it | High | Low |
| Social | Look as professional to customers as the bigger shops | Med | Med |

- **Pains we relieve:** Paperwork at night → invoice from the phone at the job; slow payment → a card-payment link on every invoice; double-booking → one shared schedule
- **Gains we create:** Paid days sooner; evenings back; invoices that look professional
- **Value proposition:** Send your first professional invoice within an hour of signing up, and get paid by card, without a sales call

#### Secondary Segment: Office managers at 5–10 technician shops

- **Value proposition:** Try Tidewell on real jobs before talking to anyone; when the shop is ready, sales helps roll it out to the whole team

### Positioning

#### Positioning Statement
**For** owner-operators of small home-services shops, **who** lose evenings to scheduling and invoicing, **our** Tidewell Starter **is a** scheduling-and-invoicing app **that** gets your first invoice out within an hour. **Unlike** field-service suites that need a demo and weeks of setup, **we** let you start, set up, and pay on your own.

#### Tradeoff — What We're Choosing Not to Be
We're not the most configurable field-service platform. Starter has no custom workflows, no inventory management, and no onboarding manager. Shops that need those go to the sales-led plan.

### Competitive Landscape

#### Positioning Matrix

|  | ← Low Field-Service Depth | High Field-Service Depth → |
|--|---------------------------|----------------------------|
| **↑ High Speed to First Invoice** | Generic invoicing apps | **Tidewell Starter** (target position) |
| **↓ Low Speed to First Invoice** | Paper and spreadsheets | Field-service suites |

#### Alternative Solutions Comparison

| Alternative | Strengths | Weaknesses | Our Advantage |
|------------|-----------|------------|---------------|
| Generic invoicing apps | Cheap, quick to start | No scheduling or dispatch | Schedule and invoice in one place |
| Field-service suites | Deep features and integrations | Demo, contract, and weeks of setup; priced for 10+ technicians | Start the same day; priced for 1–4 technicians |
| Paper and spreadsheets (status quo) | Free and familiar | Slow, error-prone, no card payments | Invoice from the job site, get paid by card |

### Strategic Moat Assessment (Helmer's 7 Powers)

| Power | Applicable? | Evidence / Plan |
|-------|------------|----------------|
| Scale Economies | N | Not at this stage |
| Network Effects | N | Shops don't benefit from other shops using Tidewell |
| Counter-Positioning | Y | Suites earn on sales-led contracts and paid implementation; a $79 self-serve plan undercuts that model, so copying it would hurt them |
| Switching Costs | Y | Customer lists, job history, and invoice records build up in Tidewell over time |
| Branding | N | Not yet — Tidewell is unknown outside its customer base |
| Cornered Resource | N | None |
| Process Power | N | None yet; onboarding data could become one by Stage 3 |

### Strategic Risks & Assumptions

| # | Assumption | Confidence | How to Validate |
|---|-----------|------------|-----------------|
| 1 | Owners finish setup without a human | Med | Stage 0: unassisted setup tests with 20 shops |
| 2 | For this segment, speed of setup matters more than feature depth | Med | Stage 0: waitlist signups on "first invoice within an hour" messaging |
| 3 | Self-serve won't pull shops with 5+ technicians from sales-led plans | Med | From Stage 1: sales-led accounts from 5+ technician shops (counter-metric) |

| # | Risk | Likelihood | Impact | Mitigation |
|---|------|-----------|--------|------------|
| 1 | Trial signups are mostly unqualified (students, competitors, hobbyists) | M | M | Business phone check at signup; trial-quality scoring |
| 2 | A field-service suite launches a free tier | L | H | Compete on speed to first invoice, not features |
| 3 | Self-serve support load overwhelms Customer Success | M | H | In-app guidance first; chat for trials only, business hours |

---

## 3. Tactics

**Linked Strategy:** Primary segment (owner-operators of 1–4 technician shops) + value proposition "first invoice within an hour, no sales call"

### Marketing & Product Mix

#### Product
| Tactic | Description | Rationale | Strategic Link | Owner | Priority |
|--------|-------------|-----------|----------------|-------|----------|
| Guided setup checklist | Five in-app steps: business details, customer import, first job, first invoice, card payments | Replaces the setup calls the sales-led path relied on | Value prop: first invoice within an hour | Product Lead, Onboarding | High |
| Customer import | Import customers from CSV, then QuickBooks | Re-entering customers is the segment's top switching cost | Primary segment: switching cost | Engineering Manager, Integrations | Med |
| Trial chat support | Live chat for trial accounts, business hours only | Safety net for owners who get stuck; capped to protect CAC payback | Assumption 1: setup without a human | Customer Success Manager, Self-Serve | Med |

#### Price
`N/A — Starter stays at $79/month (see Non-Goals). This initiative changes how shops buy it, not what it costs. Revisit if activation hits target but trial-to-paid doesn't (see Control → Iteration Triggers).`

#### Incentives
| Tactic | Description | Rationale | Strategic Link | Owner | Priority |
|--------|-------------|-----------|----------------|-------|----------|
| No-card 14-day trial | Full Starter features, no card required to start | Lowers the risk of trying | Positioning: start on your own | Head of Growth | High |
| First-invoice nudges | In-app prompt plus day-2 and day-5 emails until the first invoice is sent | Drives the activation behavior directly | Value prop: first invoice within an hour | Lifecycle Marketing Manager | High |

#### Communication
| Tactic | Description | Rationale | Strategic Link | Owner | Priority |
|--------|-------------|-----------|----------------|-------|----------|
| "Built for small shops" pages and ads | Landing pages and search ads aimed at 1–4 technician owners; a waitlist until signup launches | Carries the positioning; the waitlist measures demand early | Positioning statement | Product Marketing Manager | High |
| Trade invoice templates (SEO) | Free plumbing, HVAC, and electrical invoice templates that lead into the trial | Owners search for templates before they search for software | Primary segment: current behavior | Content Marketing Lead | Med |
| Trade-association webinars | Monthly "get paid faster" webinars with regional trade associations | Reaches owners where they already learn | Primary segment | Partner Marketing Manager | Low |

#### Distribution
| Tactic | Description | Rationale | Strategic Link | Owner | Priority |
|--------|-------------|-----------|----------------|-------|----------|
| Self-serve signup and card checkout | Sign up, start a trial, and pay on the website or mobile app | The channel itself | Positioning: no sales call | Engineering Manager, Billing | High |
| Trial-to-sales handoff | Trials that add a fifth technician get offered a sales-assisted rollout | Serves the secondary segment and protects sales-led revenue | Secondary segment value prop; Assumption 3 | Sales Development Manager | Med |

### Traceability Matrix

| Tactic | → Strategic Choice | → Goal/Objective |
|--------|--------------------|------------------|
| Guided setup checklist | Value prop: first invoice within an hour | Objective 2 |
| Customer import | Primary segment: switching cost | Objective 2 |
| Trial chat support | Assumption 1: setup without a human | Objective 2 |
| No-card 14-day trial | Positioning: start on your own | Objective 1 |
| First-invoice nudges | Value prop: first invoice within an hour | Objective 2 |
| "Built for small shops" pages and ads | Positioning statement | Objective 1 |
| Trade invoice templates (SEO) | Primary segment: current behavior | Objectives 1, 3 |
| Trade-association webinars | Primary segment | Objective 1 |
| Self-serve signup and card checkout | Positioning: no sales call | Objectives 1, 3 |
| Trial-to-sales handoff | Secondary segment value prop | Objective 1 (and the sales-led counter-metric) |

### Prioritization

**Method Used:** RICE — funnel estimates exist for reach, and the tactics differ widely in effort. Reach is trial accounts (or prospects) touched per quarter at Stage 2 volume; Effort is person-months.

| Tactic | Reach | Impact | Confidence | Effort | Score | Rank |
|--------|-------|--------|------------|--------|-------|------|
| First-invoice nudges | 1,300 | 2 | 80% | 0.5 | 4,160 | 1 |
| No-card 14-day trial | 2,400 | 1 | 80% | 0.5 | 3,840 | 2 |
| "Built for small shops" pages and ads | 2,400 | 2 | 50% | 1 | 2,400 | 3 |
| Guided setup checklist | 2,400 | 2 | 80% | 2 | 1,920 | 4 |
| Self-serve signup and card checkout | 2,400 | 3 | 80% | 4 | 1,440 | 5 |
| Customer import | 1,700 | 2 | 50% | 3 | 567 | 6 |
| Trial chat support | 700 | 1 | 80% | 1 | 560 | 7 |
| Trial-to-sales handoff | 180 | 2 | 50% | 1 | 180 | 8 |
| Trade invoice templates (SEO) | 600 | 0.5 | 50% | 1 | 150 | 9 |
| Trade-association webinars | 100 | 0.5 | 50% | 1 | 25 | 10 |

RICE rank isn't build order: the trial ranks second but can't ship before signup and checkout (see Dependencies).

### Quick Wins vs. Strategic Bets

#### Quick Wins (High Impact, Low Effort)
1. First-invoice nudges — half a person-month, and testable on new sales-onboarded accounts in Stage 0, before self-serve exists
2. "Built for small shops" pages and ads — a waitlist page goes live in January and measures demand before signup ships

#### Strategic Bets (High Impact, High Effort)
1. Self-serve signup and card checkout — four person-months and a billing upgrade, but every other tactic depends on it; pays off from launch in May
2. Customer import — three person-months; removes the segment's top switching cost; CSV in Stage 1, QuickBooks in Stage 2

#### Deprioritized (Low Impact or Premature)
1. Trade-association webinars — low reach per hour of effort until the self-serve funnel converts; reconsider in Stage 3

### Dependencies

| Tactic | Depends On | Blocked By | Notes |
|--------|-----------|------------|-------|
| Self-serve signup and card checkout | Billing vendor card-checkout upgrade | Upgrade scheduled for Q1 | On the critical path |
| No-card 14-day trial | Self-serve signup and card checkout | — | Ranks high on RICE but can't ship first |
| Guided setup checklist | Self-serve signup and card checkout | — | Prototyped in Stage 0 with sales-onboarded accounts |
| Customer import (QuickBooks) | QuickBooks partner approval | Approval pending (6–8 weeks) | CSV import ships first, in Stage 1 |
| Trial-to-sales handoff | Self-serve signup; CRM routing rule | — | Sales Operations builds the routing rule |

---

## 4. Implementation

**Overall Timeline:** January 4, 2027 → December 31, 2027

### Roadmap by Stage

#### Stage 0: Discovery & Validation
| Field | Detail |
|-------|--------|
| **Objective** | Prove owners can set up without a human, and that demand exists |
| **Duration** | 8 weeks |
| **Dates** | Jan 4 → Feb 26, 2027 |
| **Entry Criteria** | Sponsor approval; UX researcher contracted |
| **Exit Criteria** | ≥ 14 of 20 test shops send a first invoice unassisted; ≥ 500 waitlist signups |
| **Go/No-Go Decision** | Unassisted setup rate and waitlist size, reviewed by the Chief Revenue Officer |

**Deliverables:**
| # | Deliverable | Owner | Due Date | Status |
|---|------------|-------|----------|--------|
| 1 | "Built for small shops" waitlist page live | Product Marketing Manager | Jan 22 | Not started |
| 2 | Unassisted setup tests with 20 small shops | UX Researcher | Feb 12 | Not started |
| 3 | First-invoice nudges tested on new sales-onboarded accounts | Lifecycle Marketing Manager | Feb 19 | Not started |
| 4 | Billing vendor card-checkout upgrade scoped and scheduled | Engineering Manager, Billing | Feb 26 | Not started |

**Key Risks:**
| Risk | Mitigation |
|------|-----------|
| Test shops aren't representative | Recruit through trade associations, not existing customers |

#### Stage 1: MVP / Prototype
| Field | Detail |
|-------|--------|
| **Objective** | Launch self-serve signup, trial, and checkout for 1–4 technician shops |
| **Duration** | 4 months |
| **Dates** | Mar 1 → Jun 30, 2027 |
| **Entry Criteria** | Stage 0 exit criteria met + billing upgrade on schedule |
| **Exit Criteria** | 100 paid self-serve accounts in June; activation ≥ 35% |
| **Go/No-Go Decision** | Paid accounts, activation, and the CAC payback trend vs. targets |

**Deliverables:**
| # | Deliverable | Owner | Due Date | Status |
|---|------------|-------|----------|--------|
| 1 | Self-serve signup and card checkout | Engineering Manager, Billing | May 15 | Not started |
| 2 | No-card 14-day trial | Head of Growth | May 15 | Not started |
| 3 | Guided setup checklist | Product Lead, Onboarding | May 15 | Not started |
| 4 | Trial chat support (business hours) | Customer Success Manager, Self-Serve | May 15 | Not started |
| 5 | CSV customer import | Engineering Manager, Integrations | Jun 1 | Not started |

**Key Risks:**
| Risk | Mitigation |
|------|-----------|
| Billing upgrade slips | Invoice the first 50 accounts manually |

#### Stage 2: Scale
| Field | Detail |
|-------|--------|
| **Objective** | Grow trial volume at low CAC and route larger shops to sales |
| **Duration** | 3 months |
| **Dates** | Jul 1 → Sep 30, 2027 |
| **Entry Criteria** | Stage 1 exit criteria met + CAC payback ≤ 9 months for the June cohort |
| **Exit Criteria** | 200 paid self-serve accounts in September; activation ≥ 45%; CAC payback ≤ 7 months |
| **Go/No-Go Decision** | Volume, activation, and payback vs. targets; sales-led accounts from 5+ technician shops holding steady |

**Deliverables:**
| # | Deliverable | Owner | Due Date | Status |
|---|------------|-------|----------|--------|
| 1 | Trial-length A/B test (14 vs. 30 days) launched | Head of Growth | Jul 1 | Not started |
| 2 | Paid search and landing pages at scale | Product Marketing Manager | Jul 15 | Not started |
| 3 | Trade invoice templates (SEO) | Content Marketing Lead | Jul 31 | Not started |
| 4 | Trial-to-sales handoff | Sales Development Manager | Aug 1 | Not started |
| 5 | QuickBooks customer import | Engineering Manager, Integrations | Aug 15 | Not started |

**Key Risks:**
| Risk | Mitigation |
|------|-----------|
| Paid search costs rise | Shift budget to SEO templates |

#### Stage 3: Optimize & Expand
| Field | Detail |
|-------|--------|
| **Objective** | Reach 250 paid accounts a month at ≤ 6-month payback, and set FY2028 scope |
| **Duration** | 3 months |
| **Dates** | Oct 1 → Dec 31, 2027 |
| **Entry Criteria** | Stage 2 exit criteria met + trial-length test fully enrolled |
| **Exit Criteria** | 250 paid self-serve accounts in December; CAC payback ≤ 6 months for the November and December cohorts |

**Deliverables:**
| # | Deliverable | Owner | Due Date | Status |
|---|------------|-------|----------|--------|
| 1 | Trial-length readout and decision | Head of Growth | Nov 15 | Not started |
| 2 | Setup-checklist refinements from activation data | Product Lead, Onboarding | Nov 30 | Not started |
| 3 | Trade-association webinar pilot | Partner Marketing Manager | Dec 1 | Not started |
| 4 | FY2028 proposal: self-serve for 5–10 technician shops? | Head of Growth | Dec 15 | Not started |

### RACI Matrix

| Workstream | Responsible | Accountable | Consulted | Informed |
|-----------|------------|-------------|-----------|----------|
| Self-serve signup and billing | Engineering Manager, Billing | Head of Growth | Finance Business Partner, Growth | Chief Revenue Officer |
| Onboarding experience | Product Lead, Onboarding | Head of Growth | UX Researcher; Customer Success Manager, Self-Serve | Sales Development Manager |
| Acquisition (pages, ads, SEO) | Product Marketing Manager | Head of Growth | Content Marketing Lead | Chief Revenue Officer |
| Sales handoff | Sales Development Manager | VP Sales | Head of Growth | Customer Success Manager, Self-Serve |

### Dependency Map

```
Billing upgrade ──→ Signup + checkout ──→ No-card trial ──→ Paid self-serve accounts
                                                         ↗
Setup tests ──→ Guided setup checklist ──────────────────┘

CSV import ──→ QuickBooks import   (blocked by partner approval)

Critical Path: Billing upgrade → Signup + checkout → No-card trial → Paid self-serve accounts
```

### Resource Requirements

| Role / Skill | Stage | Effort (hrs/wk) | Source | Notes |
|-------------|-------|-----------------|--------|-------|
| Full-stack engineers (2) | Stage 1–2 | 80 | Internal | Signup, checkout, setup checklist |
| Integrations engineer | Stage 1–2 | 40 | Internal | CSV, then QuickBooks import |
| UX researcher | Stage 0 | 20 | Contract | Unassisted setup tests |
| Lifecycle marketer | Stage 0–3 | 20 | Internal | Nudges and trial emails |
| Content writer | Stage 2 | 20 | Contract | Trade invoice templates |
| Chat support agent | Stage 1–3 | 40 | Hire | Trial chat, business hours |

### Risk Register

| # | Risk | Likelihood | Impact | Mitigation | Owner | Status |
|---|------|-----------|--------|------------|-------|--------|
| 1 | Billing upgrade slips past Q1 | M | H | Weekly vendor check-in; invoice the first 50 accounts manually | Engineering Manager, Billing | Open |
| 2 | Trials are mostly unqualified | M | M | Business phone check at signup; trial-quality scoring | Head of Growth | Open |
| 3 | Support load breaks the CAC math | M | H | Chat for trials only, business hours; tickets per new account tracked as a health metric | Customer Success Manager, Self-Serve | Open |
| 4 | Shops with 5+ technicians choose self-serve over sales-led plans | L | H | Handoff at the fifth technician; sales-led accounts tracked as a counter-metric | VP Sales | Open |
| 5 | QuickBooks partner approval is delayed | M | L | Ship CSV import first | Engineering Manager, Integrations | Open |

### Key Milestones

| Milestone | Date | Stage | Success Criteria |
|-----------|------|-------|-----------------|
| Unassisted setup proven | Feb 26, 2027 | Stage 0 | ≥ 14 of 20 test shops send a first invoice unassisted |
| Self-serve launch | May 15, 2027 | Stage 1 | Signup, trial, and checkout live |
| Activation target met | Sep 30, 2027 | Stage 2 | ≥ 45% of trials send a first invoice within 7 days (Objective 2) |
| Channel at target | Dec 31, 2027 | Stage 3 | 250 paid accounts a month at ≤ 6-month payback (Objectives 1 and 3) |

---

## 5. Control

**Linked Goals:** Objective 1 (new paid self-serve accounts per month), Objective 2 (activation rate), Objective 3 (CAC payback)

### North Star Metric

| Field | Value |
|-------|-------|
| **Metric** | Weekly invoicing accounts |
| **Definition** | Paid self-serve accounts that sent at least one invoice in the trailing 7 days |
| **Source** | Product analytics (`invoice_sent`) joined to billing |
| **Owner** | Head of Growth |
| **Baseline** | 0 |
| **Target** | 1,000 by Dec 31, 2027 |
| **Cadence** | Weekly |
| **Action Trigger** | Declining for 2 weeks → Head of Growth reviews the funnel; declining for 4 weeks → reconvene Strategy |

### Metric Framework

#### Input Metrics (Leading Indicators)

| Metric | Definition | Source | Owner | Baseline | Target | Cadence | Action Trigger |
|--------|-----------|--------|-------|----------|--------|---------|----------------|
| Trial starts | New trial accounts per week | Product analytics | Head of Growth | 0 | 200/week by Sep 30, 2027 | Weekly | < 120/week for 2 weeks after launch → review the acquisition mix |
| Activation rate | % of trials that send a first invoice within 7 days of signup | Product analytics | Product Lead, Onboarding | 31% (sales-onboarded proxy) | 45% by Sep 30, 2027 | Weekly | < 35% for 2 weeks → review setup-checklist drop-off |
| Trial-to-paid conversion | % of trials that become paid accounts within 45 days of starting | Billing system | Head of Growth | N/A (new) | 30% by Dec 31, 2027 | Weekly | < 20% for 4 weeks → review the trial-length test and pricing page |
| New paid self-serve accounts | Trials that converted to paid with no sales touch, per month | Billing + CRM | Head of Growth | 0 | 250/month by Dec 31, 2027 | Monthly | Below the stage exit target → stage go/no-go review |

#### Health Metrics (Guardrails)

| Metric | Definition | Source | Owner | Baseline | Acceptable Range | Cadence | Action Trigger |
|--------|-----------|--------|-------|----------|-----------------|---------|----------------|
| CAC payback | (Acquisition + onboarding cost) ÷ monthly revenue per new account, by monthly cohort | Finance model | Finance Business Partner, Growth | ~15 months (sales-led, 1–4 technicians) | ≤ 9 months (Jun), ≤ 7 (Sep), ≤ 6 (Dec 2027) | Monthly | Above range for a cohort → pause paid search, shift budget to SEO templates |
| Support tickets per new account | Tickets in an account's first 30 days | Help desk | Customer Success Manager, Self-Serve | 2.1 (sales-onboarded accounts) | ≤ 1.5 | Weekly | > 2.0 for 2 weeks → add in-app guidance, review chat coverage |
| Payment failure rate | Failed card charges ÷ attempted charges | Billing system | Engineering Manager, Billing | N/A (new) | ≤ 3% | Weekly | > 5% in a week → billing incident review |

#### Counter-Metrics (Watching for Harm)

| Metric | Definition | Why It Matters | Source | Owner | Baseline | Acceptable Range | Cadence | Action Trigger |
|--------|-----------|---------------|--------|-------|----------|-----------------|---------|----------------|
| Sales-led accounts from 5+ technician shops | New sales-led accounts from shops with 5+ technicians, per month | Self-serve could pull these higher-value deals away from sales (Assumption 3) | CRM | VP Sales | 45/month | ≥ 40/month | Monthly | < 40 in a month → tighten the handoff rule, review Starter limits |
| 90-day self-serve churn | % of paid self-serve accounts that cancel within 90 days | Cheap acquisition can fill the funnel with shops that don't stay, which breaks CAC payback | Billing system | Customer Success Manager, Self-Serve | N/A (new) | ≤ 8% | Monthly | > 12% for a cohort → exit interviews, review setup |

### Goal KPI Coverage Check

| Goal (from Phase 1) | KPI | Covered in Framework? | Metric Name |
|---------------------|-----|----------------------|-------------|
| Objective 1: Open a self-serve channel for small shops | New paid self-serve accounts per month | ✅ | New paid self-serve accounts (input) |
| Objective 2: Get trial users to value fast | Activation within 7 days | ✅ | Activation rate (input) |
| Objective 3: Keep the channel profitable | CAC payback | ✅ | CAC payback (health) |

### Dashboard Design

#### Executive Dashboard (Monthly/Quarterly)
| Position | Metric | Visualization | Notes |
|----------|--------|--------------|-------|
| Hero | Weekly invoicing accounts | Big number + 13-week trend | North Star |
| Row 1 | New paid self-serve accounts, activation rate, CAC payback | Line vs. target | The three Goal KPIs |
| Row 2 | Sales-led accounts from 5+ technician shops, 90-day churn | Bar vs. acceptable range | Counter-metrics |

#### Operational Dashboard (Weekly)
| Position | Metric | Visualization | Notes |
|----------|--------|--------------|-------|
| Hero | Trial starts | Weekly bars | |
| Row 1 | Activation rate, trial-to-paid conversion | Funnel by weekly cohort | |
| Row 2 | Support tickets per new account, payment failure rate | Line vs. threshold | |

#### Team Dashboard (Daily)
| Position | Metric | Visualization | Notes |
|----------|--------|--------------|-------|
| Hero | Setup-checklist completion by step | Step funnel | Onboarding squad |
| Row 1 | Trial chat volume, first-response time | Line | Customer Success |

### Review Cadence

| Cadence | Audience | Focus | Key Questions | Decisions Made |
|---------|----------|-------|--------------|---------------|
| Daily | Onboarding squad | Operational health | Is anything broken? Are trials stuck at a setup step? | Bug fixes, checklist tweaks |
| Weekly | Head of Growth + Product + Eng | Progress vs. plan | Are trials and activation on track? What's slipping? | Nudge and checklist changes, ad spend shifts |
| Monthly | Chief Revenue Officer, VP Sales, Finance | Strategy execution | Is payback heading to 6 months? Are sales-led accounts holding? | Budget shifts, handoff-rule changes |
| Quarterly | Executive team | Goal attainment | Are we on track for 250 a month at ≤ 6-month payback? | Stage go/no-go, FY2028 scope |

### Escalation Paths

Each metric's action trigger lives in its row above. Here the PM is the Head of Growth and the exec is the Chief Revenue Officer.

| Trigger | First Responder | Escalate to PM When | Escalate to Exec When |
|---------|-----------------|---------------------|-----------------------|
| Activation < 35% for 2 weeks | Product Lead, Onboarding | Still < 35% after 2 more weeks | < 30% for a month |
| CAC payback above range for a cohort | Finance Business Partner, Growth | Immediately | Two cohorts in a row |
| Sales-led accounts from 5+ technician shops < 40 in a month | Sales Development Manager | Immediately | Two months in a row |
| Payment failure rate > 5% in a week | Engineering Manager, Billing | Same day | Above 10% |

### Feedback Loops

#### Automated Alerts
| Alert | Trigger | Channel | Recipients |
|-------|---------|---------|-----------|
| Payment failure spike | > 5% of card charges fail in 24 hours | PagerDuty | Engineering Manager, Billing |
| Activation dip | Trailing 14-day activation < 35% | Slack | Product Lead, Onboarding; Head of Growth |
| Ticket surge | > 2.0 tickets per new account in a week | Email | Customer Success Manager, Self-Serve |

#### Learning Loops
| Signal | Insight Type | Feeds Back Into | Cadence |
|--------|-------------|-----------------|---------|
| Setup-checklist drop-off by step | Which steps still need a human | Tactics (guided setup, trial chat) | Weekly |
| Trials from 5+ technician shops | Whether larger shops want self-serve | Strategy (segment priority) | Monthly |
| CAC payback by channel | Which acquisition channels pay back | Tactics (communication mix) | Monthly |
| 90-day churn reasons | Whether we're attracting the right shops | Goals (FY2028 targets) | Quarterly |

#### Iteration Triggers
| Condition | Response |
|-----------|----------|
| North Star declining for 4+ weeks | Reconvene Strategy phase |
| Activation on target but trial-to-paid below 20% | Revisit Price (marked N/A in Tactics) |
| Sales-led accounts from 5+ technician shops < 40 for 2 months | Pause ads that reach larger shops; revisit the secondary segment |
| Stage milestone missed by >2 weeks | Reassess Implementation plan |

### Experimentation Framework

| Hypothesis | Experiment Type | Duration | Sample Size | Success Criteria | Decision |
|-----------|----------------|----------|-------------|-----------------|----------|
| A 30-day trial converts more shops than a 14-day trial without hurting payback | A/B | Enroll 13 weeks (Jul–Sep 2027); read out 45 days after the last enrollment | ~1,250 trials per arm (80% power to detect +5 points) | Trial-to-paid +5 points (25% → 30%) with CAC payback ≤ 7 months | Ship the 30-day trial / keep 14 days |

---

## Appendix

### A. Key Decisions Log
| Date | Decision | Rationale | Decision Maker |
|------|----------|-----------|---------------|
| Dec 1, 2026 | Starter pricing stays at $79/month | Isolate the channel change from a price change | Chief Revenue Officer |
| Dec 1, 2026 | Self-serve limited to 1–4 technicians at launch | Protect sales-led revenue from larger shops | Chief Revenue Officer |
| Dec 8, 2026 | RICE for tactic prioritization | Funnel estimates exist for reach | Head of Growth |

### B. Stakeholder Map
| Stakeholder | Role | Interest Level | Influence Level | Engagement Approach |
|------------|------|---------------|-----------------|-------------------|
| Chief Revenue Officer | Executive sponsor | H | H | Monthly review; quarterly go/no-go |
| VP Sales | Owns the sales-led pipeline | H | H | Monthly review; co-owns the handoff rule |
| Finance Business Partner, Growth | Owns the CAC model | M | H | Monthly payback review |
| Customer Success Manager, Self-Serve | Absorbs self-serve support load | H | M | Weekly ticket review |

### C. Glossary
| Term | Definition |
|------|-----------|
| Activation | A trial sends its first invoice within 7 days of signup |
| Self-serve account | A paid account that signed up and paid with no sales touch |
| CAC payback | Months of revenue needed to recover acquisition and onboarding cost |
| Weekly invoicing account | A paid self-serve account that sent at least one invoice in the last 7 days |
| Stage | A step in the Implementation roadmap (Stage 0–3) |

---

*Document generated via GSTIC Framework — December 14, 2026*
