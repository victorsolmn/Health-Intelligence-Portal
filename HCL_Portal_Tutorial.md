# HCL Health Intelligence Portal — End-to-End Tutorial

> **File:** `hcl_portal.html` · Open by double-clicking on Desktop  
> **Assignment:** HCL Healthcare PM Assignment — Part 1 (Dashboard Portal) + Part 2 (ML Data Products)  
> **Primary User:** Priya Sharma, Benefits Head — TechCorp India GCC

---

## Table of Contents

1. [What Was Built & Why](#1-what-was-built--why)
2. [How to Navigate the Portal](#2-how-to-navigate-the-portal)
3. [Page-by-Page Walkthrough](#3-page-by-page-walkthrough)
   - [Overview](#31-overview)
   - [Workforce Health](#32-workforce-health)
   - [Programme Utilization](#33-programme-utilization)
   - [Cost & ROI](#34-cost--roi)
   - [Risk Intelligence](#35-risk-intelligence)
   - [AI Assistant](#36-ai-assistant)
   - [Data Products (Part 2)](#37-data-products-part-2)
   - [Settings](#38-settings)
4. [Config-Driven Architecture](#4-config-driven-architecture)
5. [AI Assistant — How It Works](#5-ai-assistant--how-it-works)
6. [Key Differentiators Built In](#6-key-differentiators-built-in)
7. [Sample Data Sources](#7-sample-data-sources)
8. [Part 2 — ML Data Products Summary](#8-part-2--ml-data-products-summary)
9. [Technical Stack](#9-technical-stack)
10. [What to Review Before the Interview](#10-what-to-review-before-the-interview)

---

## 1. What Was Built & Why

### The Problem
HCL Healthcare serves large enterprise clients (GCCs, Fortune 500s in India) with two core service lines:
- **AHC** — Annual Health Checks with 50+ lab parameters
- **OHC** — On-Site Health Clinics (ICD-10-coded doctor visits, specialist referrals, pharmacy, mental wellness, physio, dental)

Their clients' **Benefits Heads** currently get data in stale PDFs, with dashboards that describe what happened but don't say what to do. They can't answer their CFO's question: *"What did we get for the ₹Y crore we spent?"*

### What Was Built
A **unified, client-facing intelligence portal** that:
- Shows employer-level (not individual employee) health intelligence
- Moves from **descriptive → diagnostic → predictive → prescriptive** insights
- Handles multiple client service combinations via **JSON config** (not custom code)
- Includes a working **AI assistant** that never sends employee medical data to cloud LLMs
- Surfaces **India-specific insights** competitors miss (dependent/family risk, undiagnosed conditions)

### Design Philosophy
Every screen follows one rule from the research: **number → trend → recommendation**. Not charts that need interpretation. Answers that drive action.

---

## 2. How to Navigate the Portal

### Opening the File
Double-click `hcl_portal.html` on the Desktop. It opens in your default browser as a local file. No internet needed for the portal logic — only the charts (Chart.js) and styling (Tailwind CSS) load from CDN.

### Layout
```
┌─────────────────┬────────────────────────────────────────┐
│                 │  Top bar: Page title + period selector  │
│   SIDEBAR       ├────────────────────────────────────────┤
│                 │                                        │
│  Client         │   Page content (scrollable)            │
│  Switcher       │                                        │
│                 │                                        │
│  Navigation     │                                        │
│  Links          │                                        │
│                 │                                        │
│  User Info      │                                        │
└─────────────────┴────────────────────────────────────────┘
```

### Client Switcher (Top of Sidebar)
This is the **config-over-customisation demo**. Switch between:

| Client | Modules Active | What Changes |
|--------|---------------|--------------|
| **TechCorp India GCC** | AHC + OHC + App + Claims + Mental + Dental + Physio | Full portal — all 8 sections visible |
| **MFG Corp (AHC Only)** | AHC only | OHC, App, Mental Health sections hidden; Cost & ROI nav hidden |
| **Finance Inc (OHC + Claims)** | OHC + Claims + Mental | AHC section hidden; different config JSON shown in Settings |

When you switch clients, notice:
- The sidebar nav items change (Cost & ROI disappears for MFG Corp since no claims data)
- The programme utilization page hides irrelevant sections
- The Settings page shows a different JSON config
- The subtitle in the top bar updates with the new client's name and employee count

---

## 3. Page-by-Page Walkthrough

### 3.1 Overview

**Purpose:** Executive summary. Everything a Benefits Head needs to brief their CHRO in 60 seconds.

**What's on this page:**

#### Hero Banner
- Shows current period and **days to insurance renewal** (127 days for TechCorp)
- Shows loss ratio status — 68% is **favourable** (below the 70% threshold that triggers premium bump)
- This framing matters: the Benefits Head is always thinking about renewal leverage

#### 5 KPI Tiles
Each tile has four things: the number, the trend vs last year, context (benchmark or target), and an **analytics maturity badge**:

| Tile | Value | Maturity Badge | Why it matters |
|------|-------|----------------|----------------|
| AHC Completion | 78% | Descriptive | Below 90% target — 1,144 employees unscreened |
| Claims PEPM | ₹2,847 | Diagnostic | 12% above peer benchmark of ₹2,420 |
| High-Risk | 1,196 (23%) | Predictive | Up from 18% — leading indicator of future claims |
| OHC Visits/100 | 42 | Descriptive | Above benchmark — good utilization |
| eNPS (Benefits) | +34 | Descriptive | Above industry median of +28 — retention signal |

#### Analytics Maturity Badges
Every insight in the portal is tagged with one of four levels:
- **Descriptive** (blue) — what happened
- **Diagnostic** (purple) — why it happened
- **Predictive** (orange) — what will happen
- **Prescriptive** (green, bordered) — what to do

This is a deliberate product decision: most competitors stop at Descriptive. The portal signals its maturity tier explicitly.

#### 3 Actions for This Month (Most Important Section)
This is the core prescriptive output. Three action cards, each with:
- Maturity badge (Prescriptive / Diagnostic / Descriptive)
- The specific finding in plain language
- Financial context (cost if ignored, estimated savings)
- A CTA button

| Action | Type | Finding | Action Button |
|--------|------|---------|---------------|
| 412 undiagnosed pre-diabetics | PRESCRIPTIVE | ₹38.4L projected cost if untreated | Launch Targeted Outreach |
| Cardiac claims ▲38%, driven by parent dependents | DIAGNOSTIC | ₹28.4L, 3 of 5 are parents on floater | Review Dependent Risk |
| Pune AHC completion at 52% | DESCRIPTIVE | 480 pending, renewal in 6 weeks | Schedule Reminder Campaign |

#### Charts
- **Claims PEPM trend** (8 quarters) — line chart with peer benchmark overlay
- **Population risk tier** — donut chart with legend showing 312 / 884 / 1,820 / 2,184 across tiers

---

### 3.2 Workforce Health

**Purpose:** Understand the health profile of the workforce — who is at risk, where the trends are moving, how you compare to benchmarks.

**What's on this page:**

#### BMI Distribution (Donut Chart)
- Underweight 3% / Normal 36% / Overweight 44% / Obese 17%
- Alert: 61% overweight or obese — above IT sector average of 44%
- Data sourced from Pazcare 2026 / IT employee Scientific Reports (2025) benchmarks

#### Abnormal Lab Parameters (Horizontal Bar Chart — This Year vs Last Year)
Parameters tracked: Total Cholesterol, Triglycerides, BP (Systolic), HbA1c, LDL, Uric Acid, TSH, ALT/SGPT

- Red alert below chart: **412 employees have HbA1c ≥6.5% with zero diabetes ICD-10 code** — this is the undiagnosed risk pool that links to the Risk Intelligence page

#### Age & Gender Profile (Grouped Bar Chart)
- Male / Female split across age bands: 20–30, 31–40, 41–50, 51–60, 60+
- Largest cohort: 31–40 male (1,140 employees) — also the highest-risk age band for metabolic syndrome in India

#### Clinical Outcomes YoY (Progress Bars)
Shows how many employees moved from abnormal → normal between AHC cycles:
- HbA1c improved: +214 employees (52% of those previously abnormal)
- BP controlled: +148 employees (44%)
- Cholesterol normalised: +91 (31%)
- BMI moved to healthy: +63 (18%)
- **LFT (fatty liver) worsening: –12 employees** — red flag, needs attention

This section directly answers: *"Are our people actually getting healthier?"*

#### Chronic Disease Prevalence vs. Benchmark
5 tiles comparing TechCorp vs Indian IT sector benchmark:
- Diabetes/Pre-diabetes: 26% vs 22% benchmark ↑ (Apollo Munich data)
- Hypertension: 31% vs 26% ↑
- Lipid abnormality: 63% vs 58% ↑
- Mental health risk: 8% vs 12% ✓ (below benchmark — positive)

---

### 3.3 Programme Utilization

**Purpose:** Show the Benefits Head what she's getting for her programme spend — utilization rates, what employees are actually coming in for, and where the gaps are.

**What's on this page:**

#### AHC Section
- 4 summary tiles: Completed (4,056), Pending (1,144), Lab parameters (50+), Cost per employee (₹893)
- **Completion by Location (Bar Chart):** Bangalore 84%, Hyderabad 81%, Chennai 79%, **Pune 52%** (red — the action from the Overview page)
- **Completion by Age Band:** Younger employees (20–30) at 71% — older employees more compliant at 92% for 60+. Insight: gamification/incentives needed for young cohort

#### OHC Section
- **Top 10 ICD-10 Conditions (Horizontal Bar):** Upper respiratory (412 visits) and back pain/MSK (324) are top. E11 (diabetes management) at 196 — signals managed chronic conditions. F32–F41 (anxiety/stress) at 163 — mental health presenting at OHC, not just EAP
- **Specialist Referral Donut:** Of 14.2% referral rate — Cardiology 28%, Orthopaedics 22%, Endocrinology 19%, Psychiatry 16%. 3.1% of referrals led to hospitalisation
- Key metric: 2,184 total OHC visits

#### Mental Wellness / EAP
- EAP utilization: 8.4% (industry benchmark for India is typically 3–5%, so this is strong)
- 437 counselling sessions, 4.2/5 satisfaction
- Top presenting issues: Stress/Anxiety 48%, Relationship issues 22%, Work-life balance 18%

#### Wellness App
- MAU: 2,840 (55% of workforce) — strong for a corporate wellness app
- DAU: 1,248 (24%)
- Challenge completion: 67%
- Active challenges: 10k steps (1,892 enrolled), Mindfulness 21-day (743), Hydration (1,104)

> **Note:** If you switch to MFG Corp (AHC Only) in the sidebar, the OHC, Mental, and App sections disappear — only the AHC section remains. This is the config-driven behaviour in action.

---

### 3.4 Cost & ROI

**Purpose:** Answer the CFO question: *"What did we spend, what did we get, and are we above or below benchmark?"*

**What's on this page:**

#### 4 Summary Tiles
| Metric | Value | Context |
|--------|-------|---------|
| Total Claims Spend | ₹14.8 Cr | ▲12% vs ₹13.2Cr last year |
| Claims Loss Ratio | 68% | Below 70% renewal threshold — favourable |
| Programme Investment | ₹2.7 Cr | AHC ₹1.1Cr + OHC ₹1.6Cr |
| Causal ROI | 1.4x | ₹1.2Cr avoided (95% CI: ₹0.8–1.6Cr) |

#### Claims PEPM Trend (Line Chart — 3 lines)
- **Actual PEPM** (red, filled): Q4 FY24 ₹2,120 → Q3 FY26 ₹2,847
- **Projected without intervention** (orange dashed): Shows what trajectory would look like without the wellness programme
- **Peer benchmark** (grey dashed): Flat at ~₹2,400 — TechCorp is above but narrowing the gap

#### Claims by Category (Donut)
Cardiac ₹28.4L · Maternity ₹22.1L · Diabetes ₹18.7L · Oncology ₹15.3L · Ortho ₹12.6L · Mental ₹6.8L · Others ₹44.1L

---

#### DIFFERENTIATOR 1: Dependent / Family Risk (Amber Box)

This is a feature **no competitor surfaces clearly** according to the research document.

**The insight:** In Indian group policies, high-cost claimants are often **parents on family floaters**, not employees. TechCorp's top 5 high-cost claimants are all dependents aged 62–74.

The box shows:
- ₹28.4L in cardiac claims — 3 of 5 are dependents
- 78 high-cost claimants (>₹5L) — 31% are dependents
- Those 78 people = 1.5% of covered lives = 31% of total cost
- Potential savings via specialist navigation: ₹4.2L

**Why it matters for the interview:** When asked "what did you build that competitors don't do?" — this is the answer.

---

#### DIFFERENTIATOR 2: Causal ROI with Confidence Intervals (Green Box)

This is the trust differentiator.

**The problem with vendor ROI numbers:** Every vendor (Springbuk, Truworth) reports participation and outcomes side-by-side and lets HR draw the causation conclusion. This is wrong — it's inflated by selection bias (healthy people self-select into wellness programmes). The Illinois Workplace Wellness Study (Jones, Molitor & Reif 2019, NBER w24229) proved this with a gold-standard RCT.

**What's shown:**
- Naïve estimate: ₹3.1Cr (struck through — labelled "inflated by selection bias")
- **DiD estimate: ₹1.2Cr saved** (95% CI: ₹0.8Cr – ₹1.6Cr)
- Programme investment: ₹2.7Cr → **ROI: 1.4x**
- Strongest cohort: Pre-diabetics 31–45 in nutrition programme (n=284) — 34% reduction in OHC escalation vs matched controls

**Why this matters for the interview:** Be ready to explain what DiD (Difference-in-Differences) is and why it's more rigorous. The honest, smaller number builds more trust with a CFO than an inflated ROI that unravels under scrutiny.

---

### 3.5 Risk Intelligence

**Purpose:** Move from reactive to proactive. Show the Benefits Head who is at risk, why, and what to do before they become high-cost claimants.

**What's on this page:**

#### Risk Tier Summary (4 Cards)
| Tier | Count | % | Definition |
|------|-------|---|-----------|
| Very High | 312 | 6% | 2+ chronic conditions + recent hospitalisation |
| High | 884 | 17% | 1 chronic condition or multiple risk factors |
| Medium | 1,820 | 35% | Borderline labs or lifestyle risk factors |
| Low | 2,184 | 42% | All labs normal, no chronic conditions |

---

#### DIFFERENTIATOR 3: Undiagnosed Condition Detection (Red Box)

This is the most technically interesting feature in the portal.

**The concept:** Find employees whose AHC lab values strongly indicate a chronic condition (diabetes, hypertension, CKD) but who have **zero corresponding ICD-10 code in OHC records or insurance claims**. This is only possible by combining both datasets.

| Condition | Count | Lab Signal | Missing Code | Projected Cost |
|-----------|-------|-----------|--------------|----------------|
| Pre-diabetes/Diabetes | 412 | HbA1c ≥6.5% | No E11 code | ₹38.4L |
| Hypertension | 298 | BP >140/90 | No I10 code | ₹22.1L |
| CKD early-stage | 187 | eGFR 45–60 | No N18 code | ₹17.3L |

**Privacy note shown in the UI:** Employer sees only aggregate counts. Individual flags go to the employee via the wellness app — never named to HR.

**Why it matters for the interview:** This is the "unlock" from combining datasets. Neither AHC alone nor claims alone can surface this. AHC alone tells you 412 people have high HbA1c. Claims alone only tracks people already diagnosed. Combined: you find the hidden iceberg.

---

#### Predicted High-Cost Claimants (Next 12 Months)
- 78 employees predicted to exceed ₹5L in next 12 months
- Projected cost: ₹4.6 Cr
- Model: LightGBM, AUC 0.89 on validation set
- Top SHAP features shown: Prior hospitalisation (0.42), HbA1c rising trajectory (0.31), Age 46–60 + cardiac ICD-10 (0.28), Parent dependent with oncology claim (0.24)

The SHAP values are shown deliberately — signals that this is a model whose predictions can be explained, not a black box.

#### High/Very-High Risk by Department (Stacked Bar)
Operations (31%) and IT Infra (29%) are highest risk departments. Root cause noted: sedentary roles, night shift patterns.

#### Care Gaps — Treatment Adherence
Cross-references AHC labs with pharmacy claims to find employees with diagnosed conditions but no corresponding medication:
- Hypertension gap: 184 employees (high BP confirmed by AHC, no antihypertensive in pharmacy claims)
- Diabetes gap: 127 (HbA1c ≥7, no metformin/insulin)
- Dyslipidemia gap: 243 (high LDL/TC, no statin)

Each has a "Nudge via App" button — the recommended intervention channel that preserves privacy.

---

### 3.6 AI Assistant

**Purpose:** Let the Benefits Head ask natural-language questions and get contextual answers — without any employee medical data leaving the company's systems.

**What's on this page:**

#### Chat Interface
Left 2/3 of the screen. Features:
- Welcome message explaining the privacy architecture
- 5 suggested question chips (click to auto-populate)
- Type-ahead input with Enter key support
- Animated "Querying semantic layer..." typing indicator
- Each AI response shows: the answer, the SQL query that was generated, and a "PHI never left perimeter" badge

#### Try These Questions
Click the chips or type these yourself:

| Question | What the AI answers |
|----------|-------------------|
| "Why did our premium go up?" | 3-part breakdown: cardiac (dependents), maternity (C-section rate), diabetes. Recommendation to target 78 high-cost claimants |
| "How many employees are high risk?" | Table of all 4 tiers with counts and percentages. Notes that top 312 account for 41% of projected spend |
| "What did we get for our wellness spend?" | Naïve vs DiD estimate. Confidence interval. Strongest-responding cohort identified |
| "Which department has the highest risk?" | Table of 4 departments. Root cause for Operations. Recommendation for OHC health camp |
| "How do we compare to peer GCCs?" | Table comparing 5 metrics. Identifies claims PEPM as outlier, AHC completion and eNPS as strengths |

#### Right Panel — Architecture Explanation
Shows the 4-step flow visually:
1. You ask in plain English
2. LLM sees: schema + question ONLY (zero employee data)
3. Query runs on your data warehouse
4. Aggregated result → LLM formats answer

Also shows the **SQL preview panel** — updates in real-time with the query that would be generated for each question. This demonstrates the text-to-SQL concept concretely.

#### Deployment Modes
- **Cloud LLM** (default): Recommended for most clients. PHI never sent — LLM only sees schema and aggregated numbers.
- **Self-hosted Llama 3.1 70B** (in client VPC): For strictest compliance clients who won't allow any cloud API calls. A single RTX 4090/5090 can serve 10–15 concurrent users.
- **Disabled**: For clients who want no AI features at all.

**Why text-to-SQL is the right answer:** The LLM round-trip carries metric definitions and aggregated answers — never row-level patient data. Even a cloud LLM is compliance-safe because PHI literally never leaves the customer's environment. This is the "Option 3: Recommended Default" from the research document.

---

### 3.7 Data Products (Part 2)

**Purpose:** This page is Part 2 of the assignment — the ML and data product proposals. Written directly into the portal as an "Innovation Roadmap" so everything is in one deliverable.

Three products are presented, each with a colour-coded header:

---

#### Product 1: High-Cost Claimant (HiCC) Prediction Engine (Red)

**What it is:** Binary classifier predicting which employees will exceed ₹5L in claims in the next 12 months.

**Buyer:** Insurers/TPAs (premium pricing, care management enrolment) + Employers (intervention targeting). Priced at ₹15–25 PEPM to insurer, ₹5–10 PEPM to employer.

**Why binary classification, not regression:** Business decisions are threshold-based — care management programs enroll the top N%. Ranking accuracy (AUC-PR) matters more than continuous cost prediction. (Maisog et al. 2019 seminal paper on this).

**Model:** LightGBM primary — faster than XGBoost on wide tabular data, native categorical handling, handles missing AHC values. Calibrated with isotonic regression for reliable probability estimates. Logistic regression as explainable fallback.

**Key features (code block shown):**
- AHC: hba1c_last, hba1c_delta_yoy, hba1c_trend_3y, egfr_last, ldl_last, bmi_trajectory
- Claims: prior_12m_cost, prior_hosp_count, chronic_dx_flags (E11/I10/I25/N18/C-codes), pharmacy_atc_codes
- India-specific: dependent_cardiac_flag, parental_age_band, family_floater_dependents_count

**Class imbalance:** scale_pos_weight + threshold tuning on PR curve (NOT SMOTE, NOT ROC-only)

**Stack:** Python + LightGBM + SHAP + MLflow + FastAPI + Evidently AI + Airflow

**MVP vs Full:** MVP = claims-only model, retrained quarterly. Full = AHC + OHC + claims joint model, rolling 3-year history, SHAP explanations, federated training, Evidently drift monitoring.

**Published benchmarks:** AUC 0.912 on 48M member dataset (Maisog 2019). German study AUC 0.883 (PLOS ONE 2023).

---

#### Product 2: Undiagnosed Chronic Disease Surfacing (Purple)

**What it is:** Find employees whose labs indicate a condition (diabetes, hypertension, CKD, NAFLD) but who have no corresponding diagnosis in claims or OHC records.

**Buyer:** Employer (intervention targeting) + Insurer (risk pricing). Uniquely enabled by AHC + claims combination.

**Problem type: Positive-Unlabelled (PU) Learning**
Standard supervised learning fails here — you can't treat unlabelled samples as negative because many are undiagnosed positives. Two-stage approach:
- Stage 1: Train XGBoost on confirmed positive cases (diagnosed from claims) to predict condition probability from AHC labs
- Stage 2: Score unlabelled population. High-probability + no diagnosis = undiagnosed candidate

**Why India specifically:** 1 in 4 men aged 31–35 have abnormal HbA1c (Pazcare 2026). ~50% of men under 35 have abnormal BP. Most are undiagnosed. The model quantifies this hidden iceberg for a specific employer's population.

**Privacy architecture:** Employer sees aggregate counts only ("412 employees"). Individual flags go to employee via wellness app — never to HR. DPDP-compliant.

**Validation metric:** Precision@K — what % of top-K flagged employees received a new diagnosis within 18 months? Target: P@100 ≥40%. Baseline (random): ~8%.

**Commercial framing:** "Early lifestyle management costs ₹2,000/year vs ₹93,000/year once diagnosed." Second revenue stream: anonymised population risk profile sold to insurer as actuarial data product.

**MVP → Full:** MVP = rule-based flagging (zero ML, immediate value). V2 = XGBoost PU model, multi-condition. Full = longitudinal progression velocity, personalised intervention recommendations.

---

#### Product 3: Causal Programme ROI Engine — Uplift Modelling (Green)

**What it is:** Rigorous causal estimation of whether wellness interventions actually reduced claims — and for which sub-groups.

**Buyer:** Benefits Head — directly answers "did our spend work?" Trust differentiator vs every competitor.

**Problem type: Causal Effect Estimation**
Three methods of increasing sophistication:

1. **Difference-in-Differences (DiD):** Compare claims change for participants vs matched non-participants. Requires parallel trends assumption. Best for ongoing programmes with a defined start date.

2. **Synthetic Control:** Construct counterfactual employer from weighted combination of other HCL clients. Ideal for new programme rollouts where no internal control group exists.

3. **Causal Forests / Uplift Modelling (EconML/DoWhy):** Estimate Conditional Average Treatment Effect (CATE) — identifies which sub-groups actually benefit. E.g., "Nutrition programme worked for pre-diabetics 31–45 but not for 46+ already on medication."

**Output for Benefits Head:**
- ATE: ₹1.2Cr avoided (95% CI: ₹0.8–1.6Cr)
- Best-response cohort: Pre-diabetics 31–45, CATE = –₹18,400/person/year
- Non-responders: Age 46+ on medication, CATE ≈ 0
- Recommendation: Double down on pre-diabetic cohort. Redirect 46+ budget to specialist navigation.

**Stack:** Python + EconML + DoWhy + CausalML + MLflow

**Federated Learning angle:** HCL serves 50+ large clients. Train uplift models across pooled populations WITHOUT pooling raw data. Each client's data stays local — only model gradients are shared (Flower framework). 10x more statistical power than any single client's dataset. This is the answer to "how do you improve the model as you onboard more clients?"

**MVP → Full:** MVP = DiD on single programme cohort, manual matching, 2-week build. Full = automated pipeline + Causal Forest + Federated training + Validation Institute-style third-party certification.

---

### 3.8 Settings

**Purpose:** Demonstrate the config-over-customisation architecture. Shows how HCL can onboard a new enterprise client as a configuration step, not a development project.

**What's on this page:**

#### Client Config JSON (Left Panel)
Live JSON entitlements config for the currently selected client. Shows all fields:
- `client_id`, `client_name`, `total_employees`
- `modules` object — each service as a boolean flag
- `ai_assistant` — enabled + deployment_mode
- `risk_model` — enabled, version, threshold
- `reporting_period`, `locations`
- `consent_flow_version`, `dpdp_consent_manager`

**Key architectural point:** The portal reads these flags at runtime. A new client = a new JSON object. No developer needed.

#### Module Entitlements (Right Panel, Top)
Visual representation of the same module flags — green (Active) vs grey (Off) for each service line. Updates when you change the client in the sidebar dropdown.

#### AI Assistant Mode (Right Panel, Middle)
Three radio options with explanations:
- Text-to-SQL (Cloud LLM) — recommended default
- Self-hosted Llama 3.1 70B in client VPC — for strict compliance
- Disabled — no AI

#### DPDP Compliance Status (Right Panel, Bottom)
Checklist of compliance items:
- ✓ Consent Manager registered with Data Protection Board
- ✓ DPIA completed
- ✓ DPO appointed (India-based)
- ✓ Data residency: India (ap-south-1)
- ⚠ Phase 3 DPDP rules (May 2027) — review pending
- ℹ Note about employment-purpose processing vs analytics resale consent

---

## 4. Config-Driven Architecture

This is the "sneaky-hard" part of the assignment — the question tests whether you've thought about productisation, not just design.

### The Problem It Solves
Different clients buy different combinations of services. If every client required custom development, the product doesn't scale. The portal needs to handle this through **configuration, not custom builds**.

### How It Works

```json
{
  "client_id": "techcorp_gcc_001",
  "modules": {
    "ahc": true,
    "ohc": true,
    "app": false,
    "claims_feed": true,
    "mental_health": true
  },
  "ai_assistant": {
    "enabled": true,
    "deployment_mode": "text-to-sql-cloud"
  }
}
```

- Page sections are shown/hidden based on module flags
- Navigation items are shown/hidden based on module flags
- KPI tiles only render if their data source module is enabled
- AI query templates are scoped to available data sources
- New client onboarding = add a new config JSON. Zero code change.

### The Semantic Layer
In production, this would use **Cube.dev** or **dbt Semantic Layer**. Metrics are defined once:

```yaml
metric: high_cost_claimant_count
dimensions: [age_band, gender, location, business_unit]
sql: ...
```

The dashboard reads metrics by name. The AI assistant also reads metrics by name. Add a new client = same metrics with their own data scope (via row-level security or schema-per-client).

---

## 5. AI Assistant — How It Works

### The Core Privacy Constraint
Many enterprise clients (especially GCCs) will not allow employee medical data to be sent to cloud LLMs (OpenAI, Gemini, Claude API) due to:
1. Data residency — data leaves India
2. Sub-processing — LLM provider may share with unvetted sub-processors
3. Training-on-data risk — even when denied contractually, audit is hard
4. Healthcare compliance team discomfort regardless of contractual protections

### The Solution: Text-to-SQL over a Semantic Layer

```
User asks question in natural language
         ↓
LLM sees: schema definitions + question (ZERO employee data)
         ↓
LLM generates: Cube/SQL query JSON
         ↓
Query executes against client's data warehouse (locally)
         ↓
Aggregated result returned to LLM
         ↓
LLM formats answer in natural language
         ↓
Answer rendered with source citations
```

**PHI never leaves the customer's environment.** The LLM round-trip carries only:
- Schema definitions (table names, column names, metric names)
- The user's question
- Aggregated numeric results (counts, averages — never row-level data)

Even using a cloud LLM (GPT-4, Claude) is compliance-safe under this architecture because protected health information literally never crosses the API boundary.

### For Strictest Compliance Clients
Self-hosted **Llama 3.1 70B** on client's own GPU infrastructure. A single RTX 4090/5090 can serve 10–15 concurrent users. Modern open-source models perform within 5–10% of GPT-4 on healthcare documentation benchmarks.

### In the Prototype
The AI assistant simulates this with pre-programmed keyword-matched responses. Each response shows:
- The natural language answer (formatted with HTML bold/tables)
- The SQL query that would have been generated
- "PHI never left perimeter" badge
- Data sources cited (fact_claims, dim_employee)

---

## 6. Key Differentiators Built In

These are the things the research document specifically flagged as competitive advantages that no current vendor surfaces well:

### 1. Dependent / Family Risk Surfacing
India's high-cost claimants are often parents on family floaters, NOT employees. The portal surfaces this explicitly on the Cost & ROI page — top 5 high-cost claimants, breakdown of dependent vs employee, and a specialist navigation recommendation. No competitor does this clearly.

### 2. Causal ROI Honesty
Lead with DiD estimates and confidence intervals. Show the naïve (inflated) number crossed out. Cite the Illinois Workplace Wellness Study. A Benefits Head burned by inflated vendor ROI will love a vendor that says "here's the rigorous estimate, here's the confidence interval, here's why it's smaller than the marketing number." Trust-building moat.

### 3. Undiagnosed Condition Detection
412 employees with diabetic lab patterns, zero diabetes claims. This is uniquely enabled by combining AHC + claims. Neither dataset alone can surface this. Quantified with a projected cost trajectory.

### 4. Analytics Maturity Labels
Every insight labelled: Descriptive / Diagnostic / Predictive / Prescriptive. Makes the portal's sophistication legible to a non-technical user and signals the roadmap.

### 5. Text-to-SQL AI Architecture
Not just "AI assistant" — but a specific, defensible privacy architecture where PHI never leaves the perimeter. Shows the difference between someone who designed an AI feature and someone who thought through compliance in a healthcare context.

### 6. Config-over-Customisation
Demonstrable client switching in the portal itself. The Settings page shows the live JSON config. This answers the architectural question the assignment is actually testing.

---

## 7. Sample Data Sources

All data in the portal is realistic, sourced from:

| Source | What it contributed |
|--------|-------------------|
| **Pazcare Employee Health Matters 2026** (77K claims, 12K checkups, 400K covered lives) | 1-in-4 men 31–35 with abnormal HbA1c; ~50% men under 35 with abnormal BP; 63% cholesterol abnormalities aged 20–35; 60%+ C-section rate |
| **Apollo Munich corporate insurance analysis** (8L customers) | Diabetes incidence 9x higher for ageing workforce; 1 in 5 employees has diabetes or hypertension; diabetes claims 90% higher than other diseases |
| **Scientific Reports 2025** (IT employees, India) | Metabolic syndrome 29.87%; 44% overweight, 17% obese; 65% low HDL; MAFLD prevalence |
| **Lancet ICMR-INDIAB + JAMA Internal Medicine** | National diabetes and hypertension prevalence trends |
| **Springbuk, Truworth, Loop Health competitive analysis** | Industry benchmark positioning |

---

## 8. Part 2 — ML Data Products Summary

Quick-reference table for interview prep:

| Product | Problem Type | Model | Key Innovation | Buyer | Stack |
|---------|-------------|-------|----------------|-------|-------|
| HiCC Prediction | Binary Classification | LightGBM (calibrated) | Family floater dependent features; India-specific risk signals | Insurer/TPA + Employer | LightGBM + SHAP + MLflow + FastAPI + Evidently |
| Undiagnosed Surfacing | PU Learning (Anomaly/Disagreement Detection) | XGBoost two-stage | AHC lab vs claims gap analysis; only possible with combined dataset | Employer + Insurer | XGBoost + PU-learn + aggregate-only employer output |
| Causal ROI Engine | Causal Effect Estimation | DiD + Causal Forests (EconML/DoWhy) | Honest CI vs inflated naive ROI; CATE per sub-group | Benefits Head (primary) | EconML + DoWhy + CausalML + Federated Learning (Flower) |

---

## 9. Technical Stack

### Portal (Part 1)
| Layer | Technology | Why |
|-------|-----------|-----|
| HTML/CSS | Tailwind CSS (CDN) | Utility-first, no build step needed |
| Charts | Chart.js 4.4 (CDN) | Lightweight, works in plain HTML |
| JavaScript | Vanilla ES6 | No framework dependency, opens in any browser |
| Architecture | Config-driven (JSON) | Demonstrates the actual product requirement |
| AI simulation | Keyword-matched responses | Working prototype without a backend |

### Production Architecture (Part 1)
| Layer | Technology |
|-------|-----------|
| Semantic layer | Cube.dev or dbt Semantic Layer |
| Data warehouse | Postgres/Snowflake (schema-per-client or RLS) |
| AI (default) | Cloud LLM via text-to-SQL (no PHI sent) |
| AI (strict) | Self-hosted Llama 3.1 70B via Ollama |
| Auth | JWT + RBAC per client + role |
| Compliance | DPDP Act 2023 / DPDP Rules 2025 |

### ML Products (Part 2)
| Component | Technology |
|-----------|-----------|
| Training | Python + LightGBM / XGBoost / EconML |
| Explainability | SHAP + ICE plots |
| Experiment tracking | MLflow |
| Serving | FastAPI in client VPC |
| Monitoring | Evidently AI (data drift + model drift) |
| Orchestration | Apache Airflow |
| Federated learning | Flower framework |
| Causal inference | DoWhy + CausalML + EconML |

---

## 10. What to Review Before the Interview

The research document says: *"We will discuss your Part 2 proposals in detail during the interview. Be prepared to defend your model choices, explain your feature selection, walk through your training pipeline, and discuss what you'd do differently with more data or compute."*

### Be Prepared to Explain

**On HiCC Prediction:**
- Why LightGBM over XGBoost? (Speed on wide tabular data, native categorical handling, better with missing values)
- Why binary classification, not cost regression? (Business decisions are threshold-based; ranking matters more than continuous prediction)
- Why calibrate with isotonic regression? (Need reliable probability estimates for threshold tuning)
- Why threshold on PR curve not ROC? (At 1.5% base rate, ROC is misleading — high AUC can still have terrible precision)
- What does scale_pos_weight do? (Tells LightGBM to weight the minority positive class more heavily)
- What SHAP values tell you for each prediction (feature contribution to the prediction for an individual employee)

**On Undiagnosed Surfacing:**
- What is PU Learning and why does standard supervised learning fail here? (Unlabelled ≠ negative — many are undiagnosed positives)
- How do you validate this model without ground truth labels? (Precision@K on employees who subsequently receive a new diagnosis)
- What's the privacy architecture? (Aggregate to employer, individual to employee only)

**On Causal ROI:**
- What is DiD and what assumptions does it require? (Parallel trends — treatment and control groups would have moved in parallel without the intervention)
- What's the difference between ATE and CATE? (ATE = average across everyone; CATE = treatment effect for a specific sub-group/individual)
- Why does selection bias inflate naïve ROI? (Healthy people self-select into wellness programmes; they'd have lower claims anyway)
- Cite the Illinois study: Jones, Molitor & Reif (2019), NBER w24229. Key finding: 95% confidence intervals ruled out 84% of previously published ROI estimates.

**On the AI Assistant Architecture:**
- Why text-to-SQL over RAG for this use case? (Text-to-SQL never needs PHI in context; RAG retrieves documents which may contain PHI)
- What is the "lost in the middle" problem? (~30% performance drop when relevant info is in the middle of long LLM contexts — argues FOR text-to-SQL over document dumping)
- What's the hallucination risk in healthcare LLM outputs? (15–30%; always cite sources, always show underlying SQL, always allow human override)
- Why self-hosted Llama for strict clients? (Data never leaves the VPC; modern 70B models are within 5–10% of GPT-4 on healthcare benchmarks)

**On the Portal Architecture:**
- How does config-over-customisation work? (JSON entitlements object per client gates all page rendering, KPI tiles, and AI query templates)
- What's the data model? (Star schema: fact_ahc_results, fact_ohc_visits, fact_claims, fact_app_engagement + conformed dimensions dim_employee, dim_employer, dim_diagnosis, dim_date)
- How do you handle multi-tenancy? (Schema-per-client for prototype; row-level security on shared tables for scale)

---

*Tutorial prepared for HCL Healthcare PM Assignment · April 2026*
