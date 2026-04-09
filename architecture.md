# Architecture & Design System — HCL Health Intelligence Portal

> UI/UX brand colours, design tokens, component patterns, and layout conventions for the portal.

---

## Design System Name

**"The Clinical Curator"** — a system built for non-technical health intelligence consumers. Principles:
- Lead with numbers, follow with narrative
- No jargon on client-facing pages
- Every insight ends with a prescriptive action
- Tonal layering (no 1px borders) — surfaces stack by depth, not outline

---

## Brand Colours

### Primary Palette

| Role | Token | Hex | Usage |
|------|-------|-----|-------|
| Primary | `primary` | `#00236f` | Nav active state, buttons, links, chart primary series |
| Primary Container | `primary-container` | `#1e3a8a` | Hero card gradients, dark backgrounds |
| On Primary | `on-primary` | `#ffffff` | Text/icons on primary backgrounds |

### Secondary Palette (Health / Positive)

| Role | Token | Hex | Usage |
|------|-------|-----|-------|
| Secondary | `secondary` | `#006c49` | Success states, live indicators, positive trends, toggle active |
| Secondary Container | `secondary-container` | `#6cf8bb` | PHI badge background, success chip backgrounds |
| On Secondary Container | `on-secondary-container` | `#00714d` | Text on secondary container |

### Tertiary Palette (Risk / Alert)

| Role | Token | Hex | Usage |
|------|-------|-----|-------|
| Tertiary | `tertiary` | `#5c0016` | High-risk labels, critical alerts |
| Tertiary Fixed | `tertiary-fixed` | `#ffdada` | Alert chip backgrounds |
| On Tertiary Fixed Variant | `on-tertiary-fixed-variant` | `#920028` | Very high risk text, danger states |

### Surface Palette

| Role | Token | Hex | Usage |
|------|-------|-----|-------|
| Surface | `surface` | `#f8f9fa` | Page/body background |
| Surface Bright | `surface-bright` | `#f8f9fa` | Same as surface |
| Surface Container Lowest | `surface-container-lowest` | `#ffffff` | Card backgrounds, modal backgrounds |
| Surface Container Low | `surface-container-low` | `#f3f4f5` | Inset sections, list item backgrounds |
| Surface Container | `surface-container` | `#edeeef` | Subtle containers, disabled backgrounds |
| Surface Container High | `surface-container-high` | `#e7e8e9` | Dividers, strong container backgrounds |

### Text Palette

| Role | Token | Hex | Usage |
|------|-------|-----|-------|
| On Surface | `on-surface` | `#191c1d` | Primary body text, headings |
| On Surface Variant | `on-surface-variant` | `#444651` | Secondary text, labels, captions |
| Outline Variant | `outline-variant` | `#c5c5d3` | Scrollbar, subtle dividers |

### Semantic Colours (Non-Tokenised)

| Meaning | Hex | Usage |
|---------|-----|-------|
| Risk — Very High | `#920028` | Very high risk tier text |
| Risk — High | `#c2410c` | High risk tier text |
| Risk — Medium | `#d97706` | Medium risk / warning states |
| Risk — Low | `#006c49` | Low risk / secondary |
| Chart Blue (AHC) | `#2563eb` | AHC-sourced feature labels |
| Chart Purple (Claims) | `#9333ea` | Claims-sourced feature labels |
| Chart Amber (Confounder) | `#d97706` | Confounder/neutral variables |
| Peer Benchmark Green | `#15803d` | DiD validated savings |

---

## Typography

**Font Family:** Inter (Google Fonts)  
**Weights used:** 300 (light), 400 (regular), 500 (medium), 600 (semibold), 700 (bold), 800 (extrabold)

| Element | Size | Weight | Token |
|---------|------|--------|-------|
| Page heading (h1) | `text-3xl` / `2.4rem` | 800 | `text-on-surface` |
| Section heading (h2) | `text-xl` | 700 | `text-on-surface` |
| Card title | `text-sm` / `text-base` | 600 | `text-on-surface` |
| Body text | `text-sm` (14px) | 400 | `text-on-surface-variant` |
| Labels / captions | `text-xs` (12px) | 400–600 | `text-on-surface-variant` |
| Micro labels | `text-[10px]` | 600–700 | Uppercase + wide tracking |
| Monospace (code/SQL) | `font-mono` `text-xs` | 400 | `#4ade80` on dark bg |

**Letter spacing for micro labels:** `tracking-widest` (`letter-spacing: 0.1em`) + `uppercase`

---

## Iconography

**Library:** Material Symbols Outlined (Google)  
**Default size:** `font-size: 18px` (via `.material-symbols-outlined`)  
**Nav icons:** `font-size: 20px` (via `.nav-icon`)  
**Variation settings:** `'FILL' 0, 'wght' 400, 'GRAD' 0, 'opsz' 20`

Common icons used:

| Icon name | Usage |
|-----------|-------|
| `dashboard` | Overview nav |
| `groups` | Workforce Health nav |
| `bar_chart` | Programme Utilization nav |
| `paid` | Cost & ROI nav |
| `shield` | Risk Intelligence nav |
| `assistant` | AI Assistant nav |
| `auto_awesome` | Data Products nav |
| `settings` | Settings nav |
| `psychology` | Mental Health / upcoming product |
| `schedule` | Wait-time / scheduling |
| `hub` | Network / fraud detection |

---

## Shadows

| Token | Value | Usage |
|-------|-------|-------|
| `shadow-ambient` | `0 10px 40px -10px rgba(0,35,111,0.06)` | Default card shadow |
| `shadow-ambient-md` | `0 16px 48px -8px rgba(0,35,111,0.10)` | Elevated cards, modals |
| Data Products featured card | `0 8px 32px rgba(0,35,111,0.14)` | The "Most Impactful" middle card |

All shadows use the primary blue (`rgba(0,35,111,...)`) tint — never pure black shadows.

---

## Layout

### Shell Structure
```
<body> flex h-screen overflow-hidden
  ├── Sidebar (w-60, fixed, white, border-r)
  └── Main area (flex-1, flex-col)
        ├── Top bar (h-14, white, border-b) — breadcrumb + date + export
        └── Scrollable content (flex-1, overflow-y-auto)
              └── page-section divs (display:none / display:block via .active)
```

### Page Sections
- Each page is a `<div id="page-{name}" class="page-section p-6">` 
- Default: `display: none` | Active: `display: block` (via `.page-section.active`)
- Switched by `showPage(id, el)` JS function

### Sidebar
- Width: `240px` (fixed)
- Background: white
- Nav items: `px-3 py-2.5`, left border indicator on active (`border-left: 3px solid #00236f`)
- Client switcher: dropdown at top of nav

### Grid Conventions
- 2-column: `grid-cols-2 gap-5`
- 3-column: `grid-cols-3 gap-4` (or `gap-5`)
- 4-column KPI tiles: `grid-cols-4 gap-4`
- Data Products cards: `display:grid; grid-template-columns: 1fr 1fr 1fr; gap: 24px`

---

## Component Patterns

### Standard Card
```css
.card {
  background: white;
  border-radius: 0.75rem; /* 12px */
  box-shadow: 0 10px 40px -10px rgba(0,35,111,0.06);
}
```
Usage: `<div class="card p-5">`

### Data Products Card (new design)
```css
/* Normal card */
background: #fff;
border-radius: 16px;
border: 1px solid #e2e8f0;
box-shadow: 0 2px 12px rgba(0,0,0,0.06);

/* Featured card (middle) */
border: 2px solid #00236f;
box-shadow: 0 8px 32px rgba(0,35,111,0.14);
```

### KPI Tile
- Background: `surface-container-low` or white card
- Metric: `text-2xl font-bold text-on-surface`
- Label: `text-xs text-on-surface-variant`
- Trend badge: coloured pill (green for positive, red/amber for negative)

### Status Badge / Chip
```html
<!-- Success -->
<span style="background:#6cf8bb;color:#00714d;border-radius:9999px;font-size:11px;padding:2px 8px;font-weight:500;">
  🔒 Zero PHI · Perimeter secured
</span>

<!-- Micro label -->
<span class="text-[10px] uppercase tracking-widest bg-secondary-container text-on-secondary-container px-3 py-1 rounded-full font-semibold">
  Live
</span>
```

### Toggle Switch
```html
<label class="toggle">
  <input type="checkbox">
  <span class="slider"></span>
</label>
```
Active colour: `#006c49` (secondary)

### Nav Item (active state)
```css
background: #f3f4f5;
border-left: 3px solid #00236f;
color: #00236f;
font-weight: 600;
```

### AI Chat Bubble — User
```css
background: #00236f;
color: white;
border-radius: 1rem;
border-top-right-radius: 0.125rem;
```

### AI Chat Bubble — Assistant (Insight Card)
```css
background: #f3f4f5;
border-radius: 1rem;
border-top-left-radius: 0.125rem;
padding: 1rem;
```
Contains: answer text + inline mini bar chart (white card, 1px `#e5e7eb` border, inside the bubble)

### Insight Card Mini Chart
```css
background: #fff;
border-radius: 8px;
padding: 10px 12px;
margin-top: 10px;
border: 1px solid #e5e7eb;
```
Bar fill uses semantic risk/category colour. Bar height: `5px`, `border-radius: 3px`.

---

## Animations

| Class | Keyframe | Usage |
|-------|----------|-------|
| `.chat-msg` | `fadeUp` — `opacity 0→1`, `translateY 6px→0`, `0.25s ease` | Chat message entry |
| `.dot` | `typingBounce` — `translateY 0→-4px→0`, `1.4s infinite` | Typing indicator dots |

---

## Page-specific Design Notes

### Data Products Page
- Background: `#f8f9fb` (slightly off-white, distinct from portal surface)
- Hero header: centered, large `2.4rem` bold title, `#64748b` subtitle
- Data fusion banner: 3 columns + arrow connectors, tinted card backgrounds
- Card grid: `max-width: 1100px; margin: 0 auto` for centred layout
- Featured middle card has `position: relative` with an absolutely-positioned "Most Impactful" pill badge at `top: -13px`

### AI Assistant Page
- Right panel: white background, `#f3f4f5` for the Suggested Questions panel
- Suggestion buttons: hover state changes `background: #eef2ff; border-color: #6366f1`
- Trust pill: `background: #6cf8bb; color: #00714d`
- Collapsed query toggle: `color: #9ca3af` (grey, low-prominence)

### Risk Intelligence Page
- Risk tier colours follow the semantic palette strictly
- SHAP value displays use monospace font at `text-xs`

---

## Tailwind Config (Custom Tokens)

```js
tailwind.config = {
  theme: {
    extend: {
      colors: {
        "primary": "#00236f",
        "primary-container": "#1e3a8a",
        "on-primary": "#ffffff",
        "secondary": "#006c49",
        "secondary-container": "#6cf8bb",
        "on-secondary-container": "#00714d",
        "tertiary": "#5c0016",
        "tertiary-fixed": "#ffdada",
        "on-tertiary-fixed-variant": "#920028",
        "surface": "#f8f9fa",
        "surface-bright": "#f8f9fa",
        "surface-container-lowest": "#ffffff",
        "surface-container-low": "#f3f4f5",
        "surface-container": "#edeeef",
        "surface-container-high": "#e7e8e9",
        "on-surface": "#191c1d",
        "on-surface-variant": "#444651",
        "outline-variant": "#c5c5d3",
      },
      fontFamily: { sans: ["Inter", "sans-serif"] },
      boxShadow: {
        "ambient": "0 10px 40px -10px rgba(0, 35, 111, 0.06)",
        "ambient-md": "0 16px 48px -8px rgba(0, 35, 111, 0.10)",
      }
    }
  }
}
```

---

## External Dependencies (CDN)

| Library | Version | Purpose |
|---------|---------|---------|
| Tailwind CSS | CDN (latest) | Utility classes + custom config |
| Chart.js | 4.4.0 | All data visualisations |
| Inter | Google Fonts | Primary typeface |
| Material Symbols Outlined | Google Fonts | Icon set |

---

*Last updated: April 2026*
