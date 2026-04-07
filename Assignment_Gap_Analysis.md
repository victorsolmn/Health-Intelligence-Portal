# PM Assignment — Implementation Gap Analysis Report

> **Assignment:** HCL Healthcare Client-Facing Intelligence Portal  
> **Deliverable file:** `hcl_portal.html` (Desktop) + `HCL_Portal_Tutorial.md` (Desktop)  
> **Report date:** April 2026  
> **Legend:** ✅ Fully Implemented · 🟡 Partially Implemented · ❌ Not Implemented / Gap

---

## PART 1 — Dashboard Portal

---

### 1.1 Core Requirement: Working HTML Prototype

| Requirement | Status | Notes |
|-------------|--------|-------|
| Working HTML prototype (opens in browser) | ✅ | Single self-contained `.html` file, opens by double-clicking, no server needed |
| Populated with realistic sample data | ✅ | Uses Pazcare 2026 (77K claims, 12K checkups), Apollo Munich (8L customers), Scientific Reports 2025 IT employee study |
| Employer-level view (not individual employee) | ✅ | All metrics are population-level aggregates. Individual-level flags explicitly stated to go to employee via app only |
| Professional, usable UI | ✅ | Redesigned with "The Clinical Curator" design system — Inter font, Material Symbols icons, `#00236f` primary, tonal layering, no borders |

---

### 1.2 Page Structure & Information Architecture

| Requirement | Status | What Was Built |
|-------------|--------|----------------|
| Decide what pages it should have | ✅ | 8 pages: Overview, Workforce Health, Programme Utilization, Cost & ROI, Risk Intelligence, AI Assistant, Data Products, Settings |
| What metrics matter | ✅ | 6 metric categories implemented: Workforce Health Profile, Programme Utilization, Clinical Outcomes, Financial/Cost, Engagement, Risk |
| Insights that are actionable (not just informational) | ✅ | Every page has prescriptive action cards with CTA buttons. Analytics maturity labels (Descriptive → Prescriptive) on every insight |
| Benefits Head / HR Head as primary user | ✅ | Language, framing, and KPI selection all designed for a non-technical stakeholder reporting to CHRO/CFO |

**Pages implemented in detail:**

| Page | Key Content | Maturity Level |
|------|-------------|----------------|
| Overview | KPI tiles (5), AI Executive Summary card, prescriptive 3-action panel, PEPM trend chart, risk donut | Descriptive → Prescriptive |
| Workforce Health | BMI distribution, abnormal lab parameters chart, age/gender pyramid, YoY clinical outcomes, chronic disease vs benchmark | Descriptive + Diagnostic |
| Programme Utilization | AHC completion by location/age, OHC top 10 ICD-10, referral breakdown, mental wellness EAP, wellness app DAU/MAU | Descriptive |
| Cost & ROI | PEPM 8-quarter trend, claims breakdown, dependent risk surfacing, causal ROI with 95% CI | Diagnostic + Prescriptive |
| Risk Intelligence | Risk tier tiles, undiagnosed condition detection, HiCC predictions with SHAP values, dept risk chart, care gaps | Predictive + Prescriptive |
| AI Assistant | Working NL chat (5 Q&A pairs), text-to-SQL architecture explanation, SQL preview panel, deployment mode selector | Prescriptive |
| Data Products | 3 ML products (HiCC, Undiagnosed Surfacing, Causal ROI) with full depth — features, models, stack, MVP vs full | Strategic |
| Settings | Live JSON config, module toggles (toggle switches), AI mode selector, DPDP compliance checklist | Configuration |

---

### 1.3 Config-Driven Architecture (Different Clients, Different Service Combinations)

| Requirement | Status | Notes |
|-------------|--------|-------|
| Portal handles different service combinations via config, not custom builds | ✅ | JSON entitlements object per client. 3 demo clients: TechCorp (full suite), MFG Corp (AHC only), Finance Inc (OHC + Claims) |
| Client switcher is demonstrable | ✅ | Dropdown in sidebar — switching client updates nav items, page sections, module toggles, and JSON config display in real time |
| Page structure gated by config flags | ✅ | OHC section, App section, Mental Health section, Cost & ROI nav item — all hide/show based on module flags |
| New client onboarding = config step (not dev work) | ✅ | Adding one JSON object to `CLIENT_CONFIGS` object adds a new client with no HTML changes needed |
| Data model explanation | 🟡 | Star schema (fact_ahc_results, fact_ohc_visits, fact_claims, fact_app_engagement + conformed dimensions) documented in Tutorial.md but **not visually shown as a diagram in the portal itself** |
| How semantic layer makes config work | 🟡 | Explained in AI Assistant page and Tutorial.md. Cube.dev / dbt semantic layer is referenced but no schema diagram is embedded in the portal |

**Gap:** The assignment asks "How does the page structure and data model work so that onboarding a new client is a config step?" — this is answered functionally in the portal (the switcher works) and in writing in the Tutorial. However, a **data model diagram or architecture flow diagram** embedded in the portal (e.g., in Settings) would make this answer more explicit and visually compelling for reviewers.

---

### 1.4 Embedded AI Assistant

| Requirement | Status | Notes |
|-------------|--------|-------|
| AI assistant embedded in portal | ✅ | Full chat interface on dedicated AI Assistant page |
| Natural-language Q&A about data | ✅ | 5 pre-programmed question pairs with contextual answers |
| Working prototype | 🟡 | Keyword-matched simulation — not connected to a real SQL engine or LLM API. Accurately mimics the UX and architecture but doesn't execute live queries |
| Hard constraint: PHI must NOT be sent to cloud LLMs | ✅ | Text-to-SQL architecture implemented: LLM sees only schema + question, never row-level data |
| Research on production solution | ✅ | 4 options documented (Cloud with BAA, Self-hosted Llama, Text-to-SQL over semantic layer, Hybrid RAG) |
| Recommendation included | ✅ | **Recommended: Text-to-SQL over semantic layer** — "PHI never leaves the perimeter" even with cloud LLM. Self-hosted Llama 3.1 70B as fallback for strict clients. Configurable per client |
| SQL query shown to user | ✅ | SQL preview panel updates in real time for each question. "Zero PHI sent" badge on every response |
| Deployment mode selector | ✅ | Cloud LLM / Self-hosted Llama 3.1 70B / Disabled — configurable in Settings |

**Gap on AI Assistant:** The prototype is a simulation. The assignment says "build a working prototype" — interpretable as either a realistic UX simulation (what we built) or an actual API-connected system. For a PM assignment, a UX simulation with architecture documentation is the appropriate deliverable. However, if the interviewer expects an actual LLM call, that is a gap.

**What the prototype correctly demonstrates:**
- The UI/UX flow a Benefits Head would experience
- The text-to-SQL privacy architecture
- SQL generation and display
- Deployment mode configurability
- The "PHI never left perimeter" guarantee

**What it does NOT demonstrate:**
- Live LLM API call (OpenAI/Claude/Llama)
- Real SQL execution against a database
- Dynamic query generation from actual schema

---

### 1.5 Actionable vs Informational

| Requirement | Status | Notes |
|-------------|--------|-------|
| Insights must be actionable, not just informational | ✅ | Every page ends with or contains a prescriptive action block with CTA buttons |
| Recommendations with context | ✅ | Each action card shows: the finding, the financial implication, and an action button |
| Non-technical user language | ✅ | No model jargon on client-facing pages. SHAP, LightGBM etc. only appear in Data Products (Part 2) page which is an internal/analytical view |

**Specific actionable features implemented:**
- Overview: 3 prescriptive action cards with "Launch Outreach", "Review Dependent Risk", "Schedule Campaign" buttons
- Workforce: "Nudge via App" buttons per care gap
- Utilization: "Trigger Email Campaign", "Request Physio Staff", "Review Counsellor Availability" per alert
- Risk Intelligence: "Launch Targeted Outreach" for undiagnosed conditions
- Cost & ROI: "Review Dependent Strategy", "View Methodology" on insight cards

---

## PART 2 — Data Products

---

### 2.1 Overall Scope

| Requirement | Status | Notes |
|-------------|--------|-------|
| Propose data products from combined AHC + claims dataset | ✅ | 3 products proposed: HiCC Prediction, Undiagnosed Chronic Disease Surfacing, Causal ROI Engine |
| "Think beyond health score" | ✅ | Explicitly skipped the obvious "composite health score." Went to PU Learning, Causal Forests, LightGBM |
| 2–3 products with real depth | ✅ | 3 products implemented. Each covers all 6 required dimensions |
| What predictive models become possible | ✅ | HiCC (binary classification), Undiagnosed (PU learning), Causal ROI (DiD + Uplift), plus brief mentions of Cox survival, K-means clustering, mental health early warning |
| What insights neither dataset could produce alone | ✅ | Each product explicitly states what AHC alone vs claims alone misses, and what the combination unlocks |

---

### 2.2 Depth Per Product — Required Dimensions

#### Product 1: High-Cost Claimant (HiCC) Prediction Engine

| Required Dimension | Status | What Was Provided |
|-------------------|--------|-------------------|
| What is it and who pays? | ✅ | Binary classifier for top-1.5% cost employees. Buyers: insurer/TPA (primary), employer (secondary). Priced ₹15–25 PEPM to insurer, ₹5–10 PEPM to employer |
| Specific AHC + claims features | ✅ | Full feature list: hba1c_last, hba1c_delta_yoy, hba1c_trend_3y, egfr_last, ldl_last, bmi_trajectory, prior_12m_cost, prior_hosp_count, chronic_dx_flags (E11/I10/I25/N18/C-codes), pharmacy_atc_codes, India-specific: dependent_cardiac_flag, parental_age_band |
| Model type and why | ✅ | LightGBM (primary) — justified over XGBoost (speed, native categorical, missing value handling). Logistic regression as explainable fallback. Isotonic regression for calibration. Binary classification justified over regression (threshold-based decisions) |
| Frameworks & platforms | ✅ | Python + LightGBM + SHAP + MLflow + FastAPI + Evidently AI + Airflow |
| Training, serving, monitoring | ✅ | FastAPI in client VPC for serving. Evidently AI for drift + model performance monitoring. Airflow for retraining pipeline (monthly) |
| Privacy and consent | 🟡 | DPDP employment-purpose carve-out mentioned. Consent for analytics resale noted. **Missing:** specific mechanics of how AHC data (employer-held) and claims data (insurer-held) are combined — this is a real operational challenge in India where insurers typically won't share raw claims with employers |
| MVP vs full vision | ✅ | MVP: claims-only model, quarterly retraining. Full: AHC + OHC + claims joint model, rolling 3-year history, SHAP per employee, federated training |
| Published benchmarks | ✅ | Maisog et al. 2019 AUC 0.912, German PLOS ONE 2023 AUC 0.883. Target: ≥0.85 for Indian dataset |

#### Product 2: Undiagnosed Chronic Disease Surfacing

| Required Dimension | Status | What Was Provided |
|-------------------|--------|-------------------|
| What is it and who pays? | ✅ | PU Learning model to find employees with diabetic/hypertensive lab patterns and zero corresponding claims diagnosis. Employers pay for intervention targeting; insurers pay for actuarial risk pricing |
| Specific AHC + claims features | ✅ | hba1c, fasting_glucose, hba1c_delta, bmi, waist_circumference, family_history_diabetes, age, gender, bp_sys, bp_dia, triglycerides, hdl, egfr, creatinine, alt, ast, ggt |
| Model type and why | ✅ | Two-stage PU Learning — XGBoost trained on confirmed positives (claims-diagnosed) to predict condition probability from AHC labs. Correctly identifies why standard supervised learning fails (unlabelled ≠ negative) |
| Frameworks & platforms | ✅ | XGBoost + PU-learn library + MLflow. Aggregate-only employer output |
| Training, serving, monitoring | 🟡 | Training pipeline described. **Missing:** explicit serving architecture (batch vs real-time? scheduled AHC-cycle rerun?) and monitoring for concept drift as diabetes prevalence in population changes |
| Privacy and consent | ✅ | Strong: employer sees aggregate counts only. Individual scores go to employee via wellness app only — never to HR. DPDP-compliant |
| MVP vs full vision | ✅ | MVP: rule-based (HbA1c ≥6.5% + no E11 code) — zero ML, immediate value. V2: XGBoost PU model, multi-condition. Full: longitudinal progression velocity, personalised interventions |
| Validation metric | ✅ | Precision@K — top-K flagged employees who received new diagnosis within 18 months. Target P@100 ≥ 40% vs baseline 8% |

#### Product 3: Causal Programme ROI Engine

| Required Dimension | Status | What Was Provided |
|-------------------|--------|-------------------|
| What is it and who pays? | ✅ | Rigorous causal estimation of wellness programme impact using DiD + Causal Forests. Buyer: Benefits Head — answers "did our spend work?" Trust/credibility differentiator |
| Specific AHC + claims features | ✅ | Treatment indicator (programme_enrolled, enroll_date), outcomes (claims_12m_post, hosp_count_post, sick_days_post), confounders (age, gender, baseline_hba1c, baseline_bmi, prior_12m_claims, chronic_condition_flags, BU, location) |
| Model type and why | ✅ | Three methods: DiD (parallel trends, ongoing programmes), Synthetic Control (new rollouts, no control group), Causal Forests/EconML (CATE per sub-group). Illinois Wellness Study (Jones, Molitor & Reif 2019) cited to defend rigour |
| Frameworks & platforms | ✅ | Python + EconML + DoWhy + CausalML + MLflow. Results: ATE + CATE heatmap + bootstrap CI |
| Training, serving, monitoring | 🟡 | Training described. **Missing:** how often is the ROI recalculated? (semi-annual? post-programme cycle?) and what triggers a re-run (new AHC data? new claims period?) |
| Privacy and consent | 🟡 | DiD uses aggregated cohort-level data. **Missing:** explicit discussion of consent challenges when combining insurer-held claims with employer-held AHC — data sharing agreement mechanics |
| MVP vs full vision | ✅ | MVP: manual DiD, 2-week build. V2: Synthetic Control for new clients. Full: Causal Forest CATE + Federated + Validation Institute certification |
| Federated Learning angle | ✅ | Flower framework for cross-client model training without pooling raw data. Correctly positioned as the answer to "how do you improve models with new client data while respecting data residency" |

---

### 2.3 Cross-Cutting Part 2 Requirements

| Requirement | Status | Notes |
|-------------|--------|-------|
| Model architecture | ✅ | LightGBM architecture, PU learning two-stage, Causal Forests — all described with justification |
| Feature engineering | ✅ | Feature lists with data source annotations. Delta/trajectory features (hba1c_delta_yoy, hba1c_trend_3y). India-specific features (family floater dependents) |
| Deployment pipeline | 🟡 | FastAPI + Airflow described. Missing: containerisation (Docker/K8s), CI/CD pipeline for model updates, A/B testing framework for model versions |
| Commercial positioning | 🟡 | Buyers identified (employer, insurer/TPA, Benefits Head). Pricing model mentioned for HiCC. **Missing:** detailed go-to-market, competitive pricing analysis vs Springbuk/Truworth/Ekincare, revenue model breakdown |
| Data privacy + consent (combining datasets) | 🟡 | DPDP 2023 coverage, employment-purpose carve-out, consent for analytics resale. **Gap:** The specific operational challenge of the insurer holding claims data and not sharing it with the employer (common in India) needs explicit addressing — data access agreement structure, TPA as neutral intermediary, anonymisation pipeline |
| "Interview-ready" depth | 🟡 | Sufficient to answer most model choice and feature questions. Would benefit from: (a) class imbalance handling detail for PU learning, (b) feature importance analysis, (c) what changes with more data/compute |

---

## EVALUATION CRITERIA — Assignment's "What We Care About"

| Criterion | Status | Assessment |
|-----------|--------|------------|
| **Product instinct** — did you build something a Benefits Head would actually use? | ✅ | Yes. Portal leads with a number → trend → recommendation on every screen. Language is non-technical. Actions are real and specific (not "explore more"). Renewal leverage surfaced prominently. India-specific framing (₹ amounts, dependent risk, DPDP compliance) |
| **Depth of AI and ML thinking** — evidence you've worked with these tools | 🟡 | Strong on model selection (LightGBM over XGBoost justification, PU Learning rationale, CATE vs ATE), feature engineering, and evaluation metrics. Moderate depth on deployment/monitoring and MLOps. Could be stronger on training pipeline specifics and what you'd do differently with more compute |
| **Ambiguous brief → concrete output** | ✅ | Went from a 2-page brief to 8 working pages, 3 ML products, a design system, config architecture, and a 21-page tutorial |
| **AI tool usage** | ✅ | Claude Code used for the entire build — HTML prototype, design system implementation, tutorial generation, gap analysis. The research document (hcl assignment.pdf) demonstrates pre-build research. This document is itself generated with AI tooling |
| **Thought beyond the brief** | ✅ | Several "beyond the brief" additions: dependent/family risk surfacing (India-specific, no competitor does this), causal ROI with honest CI vs inflated naive ROI, undiagnosed condition detection via PU learning, federated learning angle for cross-client models, DPDP Act 2023 employer carve-out nuance, Validation Institute-style certification mentioned |

---

## SUMMARY SCORECARD

| Category | Score | Key Gaps |
|----------|-------|----------|
| HTML Prototype — core build | 9/10 | AI assistant is a simulation, not live |
| Page structure & metrics selection | 10/10 | All relevant metrics surfaced |
| Actionable vs informational | 9/10 | Strong prescriptive layer throughout |
| Config-driven architecture | 8/10 | Works functionally; data model diagram missing |
| AI assistant UX | 8/10 | Working simulation; text-to-SQL architecture well explained |
| AI privacy recommendation | 9/10 | Correct recommendation (text-to-SQL) with alternatives |
| Part 2 — Product 1 (HiCC) | 9/10 | Strong. Minor gap: AHC-claims data access mechanics |
| Part 2 — Product 2 (Undiagnosed) | 8/10 | Strong PU Learning framing. Gap: serving architecture |
| Part 2 — Product 3 (Causal ROI) | 8/10 | Best-in-class causal framing. Gap: rerun triggers, consent mechanics |
| Commercial positioning | 7/10 | Buyers identified; pricing model partial; GTM missing |
| Data privacy across all products | 7/10 | DPDP covered; insurer data access challenge underaddressed |
| **Overall** | **8.4/10** | |

---

## GAPS TO ADDRESS (Priority Order)

### HIGH PRIORITY — likely to come up in interview

**1. The AHC + Claims data combination challenge (India-specific)**
The insurer typically holds claims data in India and does NOT share raw claims with employers. This is a critical real-world constraint. The current proposal assumes you have both datasets, but doesn't explain HOW:
- *What to add:* Data access agreement structure: HCL acts as a neutral data processor under a tripartite agreement (Employer + Insurer + HCL). TPA as intermediary. Pseudonymisation before combination. DPDP consent for analytics purpose.

**2. AI assistant — production architecture clarity**
The prototype is a simulation. Be ready to explain in the interview:
- *What to add:* The exact flow — user question → LLM API call (schema only, no PHI) → Cube query JSON generated → executed on local warehouse → aggregated result returned → LLM formats answer. How hallucination is guarded against (always show source SQL, always show data, human override).

**3. ML deployment pipeline specifics**
The assignment says "walk through your training pipeline."
- *What to add:* Retraining trigger logic (drift threshold from Evidently AI, new AHC cycle data arrival), model versioning in MLflow, shadow deployment before promotion, rollback mechanism.

### MEDIUM PRIORITY — good to have

**4. Data model diagram**
A visual star schema diagram in the Settings or a separate "Architecture" section would make the config-over-customisation claim more concrete and defensible.

**5. Part 2 serving architecture for Product 2 (Undiagnosed)**
Batch scoring (annual, after each AHC cycle) vs real-time. Latency requirements for wellness app notification trigger.

**6. Commercial positioning depth**
Competitive pricing: Springbuk charges ~$3–5 PEPM in the US. India market: Truworth is at ₹10–30 PEPM range for platform access. Where does HCL's intelligence layer sit and what's the incremental value justification?

### LOW PRIORITY — polish

**7. Feature engineering pipeline diagram**
A simple pipeline diagram: raw AHC CSV → cleaning → feature computation (delta features, trajectory features) → model input matrix. 

**8. What would you do with more data/compute?**
The interviewer will ask this. Prepare: (a) more AHC cycles = longitudinal trajectory models replace cross-sectional; (b) more clients = federated learning becomes viable; (c) more compute = neural architectures (TabNet, NODE) vs tree methods, survival analysis models for time-to-event.

---

## WHAT IS NOT IN THE PORTAL AT ALL

| Missing Item | Reason | Severity |
|-------------|--------|----------|
| Real LLM API call | Requires backend + API keys | Low — simulation is appropriate for PM assignment |
| Real SQL execution | Requires database | Low — simulation is appropriate |
| Login / authentication | Out of scope for prototype | Low |
| Role-based access control | Out of scope for prototype | Low |
| Data model / architecture diagram | Not built | Medium — add to Settings or a dedicated Architecture tab |
| Export to PDF (functional) | Button exists but is non-functional | Low |
| Date range filter (functional) | Selector exists but doesn't update charts | Low |
| Mobile responsive design | Desktop-only | Low for enterprise B2B |
| Real benchmark data integration | Uses hardcoded benchmarks | Low — appropriate for prototype |
| Insurer data access agreement architecture | Not documented | High — critical for Part 2 credibility |

---

*Gap analysis prepared against the PM Assignment brief · April 2026*
