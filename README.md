# HCL Health Intelligence Portal

A client-facing employer health intelligence portal prototype built for the HCL Healthcare PM assignment.

## Files

| File | Description |
|------|-------------|
| `hcl_portal.html` | Working single-file portal prototype — open directly in any browser |
| `HCL_Portal_Tutorial.md` | End-to-end tutorial covering architecture, navigation, and all 8 pages |
| `Assignment_Gap_Analysis.md` | Requirements vs implementation gap analysis with scorecard |

## Portal Overview

The portal is built for Benefits Head / HR Head personas at employer clients. It covers:

- **Overview** — KPI snapshot with workforce health score
- **Workforce Health** — AHC trend analysis, condition prevalence, department breakdowns
- **Programme Utilization** — OHC, wellness app, dental, physio engagement
- **Cost & ROI** — Claims analysis, DiD-based programme ROI, dependent risk
- **Risk Intelligence** — HiCC prediction model output, undiagnosed condition flags
- **AI Assistant** — Text-to-SQL simulation (PHI-safe, schema-only LLM architecture)
- **Data Products** — ML product catalogue with 3 products: HiCC, Undiagnosed Detection, Programme ROI
- **Settings** — Config-driven module management per client

## Design System

Follows "The Clinical Curator" design system:
- Primary: `#00236f` | Secondary: `#006c49` | Tertiary: `#5c0016`
- Inter font, Material Symbols Outlined icons
- Tonal layering (no 1px borders), ambient blue-tinted shadows

## Tech Stack

- Pure HTML/CSS/JS (no build step)
- Tailwind CSS CDN with custom design tokens
- Chart.js 4.4 for all data visualizations
- Config-driven architecture — 3 demo clients with module entitlement gating
