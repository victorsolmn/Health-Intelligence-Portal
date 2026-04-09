# Knowledge Base — HCL Health Intelligence Portal

> Project context, decisions, data model, and feature overview for ongoing development.

---

## What This Project Is

A **client-facing employer health intelligence portal** prototype built for an HCL Healthcare PM assignment. It is a single self-contained HTML file (`hcl_portal.html`) that opens directly in any browser with no server or build step required.

**Primary persona:** Benefits Head / HR Head at a large Indian employer (GCC, manufacturing, finance).  
**Secondary persona:** CHRO, CFO reviewing population health and programme ROI.

---

## Assignment Context

The brief asked for:
1. A working HTML prototype of a health intelligence portal
2. Config-driven architecture (different clients, different module access — no custom builds)
3. An embedded AI assistant with a PHI-safe architecture
4. 2–3 ML data product proposals from combined AHC + Claims data
5. Actionable insights — not just informational dashboards

The portal scored **8.4/10** in self-assessed gap analysis. See `Assignment_Gap_Analysis.md` for full scorecard.

---

## Portal Pages (8 total)

| Page | Nav Key | Purpose | Analytics Maturity |
|------|---------|---------|-------------------|
| Overview | `overview` | KPI snapshot, AI executive summary, prescriptive actions | Descriptive → Prescriptive |
| Workforce Health | `workforce` | AHC trends, condition prevalence, department breakdowns | Descriptive + Diagnostic |
| Programme Utilization | `utilization` | AHC/OHC/Wellness/EAP engagement | Descriptive |
| Cost & ROI | `cost` | PEPM trends, claims breakdown, dependent risk, DiD ROI | Diagnostic + Prescriptive |
| Risk Intelligence | `risk` | HiCC prediction output, undiagnosed flags, SHAP values | Predictive + Prescriptive |
| AI Assistant | `assistant` | Natural language Q&A, text-to-SQL architecture | Prescriptive |
| Data Products | `dataproducts` | ML product catalogue — HiCC, Silent Disease Discovery, Causal ROI | Strategic |
| Settings | `settings` | Config-driven module management, DPDP compliance | Configuration |

---

## Data Model

### Source Datasets
- **AHC (Annual Health Checkup):** Lab values (HbA1c, lipids, BP, BMI, eGFR), vitals, questionnaire data, OHC referrals. Held by employer.
- **Claims:** ICD-10 diagnosis codes, claim amounts, hospitalisation counts, pharmacy ATC codes, PEPM. Held by insurer/TPA.
- **OHC (Occupational Health Centre):** Visit logs, ICD-10 diagnoses, referral outcomes. Held by employer.
- **Wellness App:** DAU/MAU, feature engagement, programme enrolment. Held by employer.

### Star Schema (Semantic Layer)
```
fact_ahc_results        → dim_employee, dim_location, dim_date
fact_ohc_visits         → dim_employee, dim_icd10, dim_date
fact_claims             → dim_employee, dim_icd10, dim_date, dim_provider
fact_app_engagement     → dim_employee, dim_date, dim_feature
```

### Why Combined AHC + Claims Matters
- AHC alone: lab values, no cost context, no hospitalisation prediction
- Claims alone: reactive (already sick), no early warning, cannot identify undiagnosed
- Combined: predict high-cost before it happens, surface undiagnosed conditions, measure causal programme ROI

---

## Config-Driven Architecture

All client differences are handled via a single `CLIENT_CONFIGS` JavaScript object. No HTML changes needed to onboard a new client.

### Demo Clients

| Client | Modules Active |
|--------|---------------|
| TechCorp India GCC | AHC + OHC + Claims + Wellness App + Mental Health |
| MFG Corp | AHC only |
| Finance Inc | OHC + Claims |

### Module Flags
```js
{ ahc, ohc, claims, app, mentalHealth, riskIntel, costRoi }
```
Nav items, page sections, and module toggles all gate on these flags in real time.

---

## AI Assistant Architecture

**Approach:** Text-to-SQL over a semantic layer (Cube.dev / dbt).

**Privacy guarantee:** The LLM sees only the schema + the user's question — never row-level employee data. The query runs on the client's own data warehouse and only aggregated results return.

**Flow:**
```
User question (plain English)
  → LLM API call (schema + question only — zero PHI)
  → SQL generated
  → Executed on client's local warehouse
  → Aggregated result returned
  → LLM formats answer
```

**Deployment options (configurable per client):**
1. Text-to-SQL via Cloud LLM (recommended — PHI never sent)
2. Self-hosted Llama 3.1 70B in client VPC (strictest compliance)
3. Disabled (standard reporting only)

**Current prototype:** Keyword-matched simulation across 5 Q&A pairs. Not connected to a live LLM or database — appropriate for a PM assignment prototype.

---

## Data Products (3 live)

### 1. HiCC Predictor
Binary classifier for top 1.5% high-cost claimants (>₹5L / 12 months).  
**Model:** LightGBM + SHAP | **AUC:** 0.89 | **Retrain:** Monthly via Airflow  
**Buyers:** Insurer/TPA (₹15–25 PEPM), Employer (₹5–10 PEPM)

### 2. Silent Disease Discovery
PU Learning model finding employees with diabetic/hypertensive AHC patterns and zero ICD-10 claims.  
**Model:** Two-stage XGBoost PU Learning | **Metric:** Precision@100 ≥ 40% vs 8% baseline  
**Key stat:** 412 employees at TechCorp = ₹38.4L projected exposure

### 3. Causal ROI Engine
DiD + Causal Forests to rigorously measure wellness programme savings (not naïve before/after which inflates ROI 2.5×).  
**Methods:** DiD, Synthetic Control, Causal Forests/CATE (EconML) | **Result:** ₹1.2Cr vs ₹3.1Cr naïve  
**Citation:** Jones, Molitor & Reif (2019) Illinois Workplace Wellness Study

---

## India-Specific Considerations

- **Data access challenge:** Insurers in India typically do not share raw claims with employers. HCL acts as a neutral data processor under a tripartite DPA (Employer + Insurer + HCL). SHA-256 pseudonymised join before any cross-dataset operation.
- **DPDP 2023:** Employment-purpose carve-out for health analytics processing. Consent for analytics resale is a separate layer. Data residency: India (ap-south-1, Mumbai).
- **Family floaters:** Parent dependents (age 62–71) on family floater policies are a significant driver of cardiac costs — a feature unique to Indian health insurance that non-India models omit.
- **South Asian thresholds:** BMI ≥23 (not 25) for overweight; HbA1c thresholds calibrated to South Asian prevalence rates.

---

## Key Product Decisions

| Decision | Rationale |
|----------|-----------|
| Single HTML file | No server needed — opens by double-click, shareable as email attachment |
| LightGBM over XGBoost | Native categorical (ICD-10), native missing values (AHC gaps), 3–5× faster |
| PU Learning for undiagnosed | "No claim" ≠ healthy — it's a missing label, not a negative |
| DiD over naïve before/after | Illinois Wellness Study shows naïve methods inflate ROI 2.5× |
| Text-to-SQL over RAG | PHI never leaves perimeter even with cloud LLM |
| Tailwind + Chart.js CDN | Zero build tooling — prototype can be modified in any text editor |

---

## Known Gaps (Priority Order)

1. **AHC + Claims data combination mechanics** — tripartite DPA operational details not fully documented in portal
2. **AI assistant is a simulation** — no live LLM API call or real SQL execution
3. **ML deployment pipeline** — Docker/K8s containerisation, CI/CD, A/B testing framework not described
4. **Data model diagram** — star schema not visually embedded in portal (only in this doc)
5. **Commercial pricing depth** — competitive analysis vs Springbuk/Truworth/Ekincare missing

---

*Last updated: April 2026*
