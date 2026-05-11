# TIPOLIS UI SPEC (Canonical)

> **Source of visual truth — non-negotiable:** `general/index.html` and its published preview at `https://rafaleandrog.github.io/tipolis-sandbox/general/`.
>
> Every visual decision in this repository is reverse-engineered from that page. When this document and the page disagree, **the page wins** and this document is updated to match. Do not refactor the page to match a previous version of this document.
>
> **Mandatory rule for the entire repository:** every HTML interface must inherit this language by loading the two shared stylesheets and the three Google Fonts families below.
>
> ```html
> <link rel="preconnect" href="https://fonts.googleapis.com">
> <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
> <link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Mono:wght@400;500&family=Jost:wght@300;400;500;600;700&family=DM+Sans:wght@400;500;700&display=swap" rel="stylesheet">
> <link rel="stylesheet" href="../assets/css/tipolis-design-system.css">
> <link rel="stylesheet" href="../assets/css/style.css">
> ```
>
> (Use `./assets/...` only for root-level pages.)
>
> **UI contract:** Any new page, card, dashboard, chart, calculator, or text section that does not follow these tokens, components, and theme accents is considered out of spec and must be refactored before merge.

---

## 0. Quick Anatomy of General

`general/index.html` is a React single-file app composed of:

- A **sticky top navigation bar** with a logo and 5 pill-style topic switchers, each colored by the topic accent.
- Five topic pages — **Overview, STRC Treasury, Digital Money, City Bonds, Strategic Decision** — each carrying its own accent color (see §3.3).
- A repeating section pattern: **DM Mono eyebrow label → Bebas Neue display heading → body paragraph → content blocks (cards / tables / charts / interactive widgets)** separated by hairline rules.
- A consistent dark navy backdrop (`#0D1E35` body, deeper `#0A1828` content) with **layered card depths** (`#003050` → `#002A45` → `#002040` → `#001E35` → `#001829`) instead of light surfaces.
- A rich set of bespoke components — interactive calculator, allocation slider, market funnel SVG, circular flywheel, actor flow diagrams, NPD stage tracker, pattern cards, callout boxes — every one following the same token system.

Everything below codifies that anatomy.

---

## 1. Guiding Aesthetic

Tipolis presents itself as a **strategic capital institution with editorial precision** — sovereign-wealth-fund seriousness, financial-publication discipline, and the visual energy of a modern terminal.

| Dimension | Direction |
|---|---|
| Tone | Authoritative, kinetic, decisive. Never decorative. |
| Hierarchy | Three sharp tiers: monospace eyebrow → condensed display title → quiet body. |
| Surface | Dark navy only. Five depth tiers stack like sediment. Light surfaces appear only inside specific actor diagrams. |
| Accent usage | **Each page/topic carries one accent.** Within a page, secondary accents may appear only to mark explicit semantic roles (success, risk, peer, reference). |
| Spacing | Tight at the unit level (8–18px), generous at the section level (36–80px). |
| Motion | Functional only — counters animate, sliders react, sections fade in. No bounce, no rotation, no scale. |
| Density | High. Information is the product. Cards never have empty corners. |

The brand does **not** use: pastel palettes, brand mascots, rounded pill buttons, gradient text, drop-shadowed glassmorphism, neumorphism, isometric illustrations, friendly illustration, or anything that would appear in consumer SaaS marketing.

---

## 2. Files & Asset Conventions

```
docs/ui/TIPOLIS_UI_SPEC.md         ← this document
assets/css/tipolis-design-system.css ← design tokens + base components
assets/css/style.css               ← portal-specific helpers (cards, navigation grid)
assets/js/main.js                  ← optional small enhancers
```

**Required `<head>` block for every HTML file in the repo:**

```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Mono:wght@400;500&family=Jost:wght@300;400;500;600;700&family=DM+Sans:wght@400;500;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="../assets/css/tipolis-design-system.css">
<link rel="stylesheet" href="../assets/css/style.css">
```

---

## 3. Color System

All colors are CSS custom properties. **Never hardcode hex values in components** — always reference a token.

### 3.1 Raw palette

```css
:root {
  /* ── Surface — Deep Navy Stack ──────────────────── */
  --color-navy-1000: #0A1828;   /* Page background under content area */
  --color-navy-950:  #07111F;   /* Hero / page chrome darkest */
  --color-navy-900:  #0D1E35;   /* App body / sticky nav backdrop */
  --color-navy-850:  #001E35;   /* Form-input fill */
  --color-navy-800:  #001829;   /* Deep card fill */
  --color-navy-750:  #002040;   /* Stat tile background */
  --color-navy-700:  #002A45;   /* Mid-depth content card */
  --color-navy-650:  #003050;   /* Risk card / surface card */
  --color-navy-600:  #00264A;   /* Tabbed inner panel */
  --color-navy-500:  #1E3A5F;   /* Hover state on dark cards */
  --color-navy-400:  #274C7A;   /* Border between dark surfaces */

  /* ── Text on Dark ───────────────────────────────── */
  --color-text-on-dark:        #FFFFFF;   /* Primary, hero, stat values */
  --color-text-on-dark-2:      #C8D4E0;   /* Body copy — paragraphs, list items */
  --color-text-on-dark-muted:  #7A8A9E;   /* Muted labels, helper text */
  --color-text-on-dark-faint:  #5A6A7A;   /* Minimal — disabled, footnotes */
  --color-text-on-dark-fainter:#8A9AAE;   /* Slider min/max labels */

  /* ── Theme Accents (one per topic) ──────────────── */
  --accent-treasury:   #FF6B35;  /* Orange — STRC Treasury, Overview */
  --accent-digital:    #F5A623;  /* Gold — Digital Money / SATS */
  --accent-bonds:      #3B82F6;  /* Blue — City Bonds */
  --accent-strategic:  #2ECC71;  /* Green — Strategic Decision, success */
  --accent-tools:      #0D9E8E;  /* Teal — interactive tools, two-sided market */
  --accent-peer:       #A78BFA;  /* Purple — peer comparison, white-label */
  --accent-warning:    #E74C3C;  /* Red — risk, alerts, lock-in */
  --accent-warning-2:  #EF4444;  /* Red variant */
  --accent-cyan:       #06B6D4;  /* Cyan — razor-and-blade, growth */

  /* ── Light surfaces (used inside actor flow diagrams only) ── */
  --color-light-bg:        #F3F6FA;  /* Outer wrapper of actor-flow diagram */
  --color-light-card:      #FFFFFF;  /* Center actor card (white) */
  --color-light-card-text: #0E1633;  /* Text on light card */

  /* ── Gradients ─────────────────────────────────── */
  --gradient-actor:   linear-gradient(180deg, #2D3CFF, #24358F);    /* Investor / actor side cards */
  --gradient-hub:     linear-gradient(180deg, rgba(0,42,69,.96), rgba(0,30,53,.96));   /* Flywheel hub */
  --gradient-pattern: linear-gradient(135deg, rgba(255,107,53,.10), rgba(255,107,53,.03)); /* Pattern card (template — swap color) */

  /* ── Hairlines ─────────────────────────────────── */
  --hairline-on-dark:      rgba(255,255,255,.05);  /* Section dividers */
  --hairline-on-dark-2:    rgba(255,255,255,.08);  /* Card hairline */
  --hairline-on-dark-3:    rgba(255,255,255,.12);  /* Stronger card border */
}
```

### 3.2 Semantic tokens

```css
:root {
  --color-bg-page:        var(--color-navy-900);  /* Body */
  --color-bg-app:         var(--color-navy-1000); /* Inside container under sticky nav */
  --color-bg-card:        var(--color-navy-700);  /* Default content card */
  --color-bg-card-deep:   var(--color-navy-800);  /* Deeper / nested card */
  --color-bg-card-mid:    var(--color-navy-750);  /* Stat tile */
  --color-bg-card-surface:var(--color-navy-650);  /* Risk card surface */
  --color-bg-input:       var(--color-navy-850);
  --color-bg-input-soft:  rgba(255,255,255,.02);

  --color-text-primary:   var(--color-text-on-dark);
  --color-text-body:      var(--color-text-on-dark-2);
  --color-text-secondary: var(--color-text-on-dark-muted);
  --color-text-muted:     var(--color-text-on-dark-faint);
  --color-text-eyebrow:   var(--color-text-on-dark-muted); /* default; usually overridden by theme accent */

  --color-border-card:    var(--hairline-on-dark-2);
  --color-border-strong:  var(--hairline-on-dark-3);
  --color-divider:        var(--hairline-on-dark);
}
```

### 3.3 Theme accent mapping (per project / page)

Every page **must declare one and only one** primary accent. It scopes the eyebrow label, the active nav pill, accent borders, stat numbers, and chart highlights on that page.

| Folder / Page | Primary accent | Token |
|---|---|---|
| `general/` Overview slide | Orange | `--accent-treasury` |
| `strc-treasury/` | Orange | `--accent-treasury` |
| `digital-money/` | Gold | `--accent-digital` |
| `city-bonds/` | Blue | `--accent-bonds` |
| `general/` Strategic Decision slide | Green | `--accent-strategic` |
| `real-estate/` | Purple | `--accent-peer` |
| `infrastructure-development/` | Teal | `--accent-tools` |
| `business-development/` | Cyan | `--accent-cyan` |
| `press-research-communications/` | Gold | `--accent-digital` (editorial) |
| Generic landing (root `index.html`) | Orange | `--accent-treasury` |

Declare the page accent at the top of the page scope:

```css
.page--digital-money {
  --accent: var(--accent-digital);
  --accent-bg-10: rgba(245,166,35,.10);
  --accent-bg-06: rgba(245,166,35,.06);
  --accent-border-20: rgba(245,166,35,.20);
  --accent-border-30: rgba(245,166,35,.30);
}
```

All component classes below reference `var(--accent)` rather than a specific hue.

### 3.4 Color usage rules

- **Default surface = navy.** White or light backgrounds are reserved for the inner actor card of a flow diagram (§9.18) and nothing else.
- **One primary accent per page.** Secondary accents are only allowed to mark explicit semantic roles: `--accent-strategic` for "validated/complete", `--accent-warning` for "risk/blocker", `--accent-peer` for "comparison/reference".
- **Tinted backgrounds use the accent at 6–11% opacity** (`{color}10` / `{color}11` in inline form). Borders use the same accent at 20–30% opacity.
- **Eyebrow labels and stat numbers are colored** with the active accent. Card titles use white or the accent; body paragraphs always use `--color-text-body` (`#C8D4E0`).
- Never combine three accent colors in the same card.

---

## 4. Typography

### 4.1 Font families

```css
:root {
  --font-display: 'Bebas Neue', 'Oswald', 'Impact', system-ui, sans-serif;
  --font-body:    'Jost', 'DM Sans', system-ui, sans-serif;
  --font-mono:    'DM Mono', 'JetBrains Mono', 'IBM Plex Mono', monospace;
}
```

- **Bebas Neue** is the display face: condensed, all-caps by nature, used for hero titles, section headings, stat values, key labels inside diagrams. Never use it for body copy.
- **DM Mono** is the label face: every eyebrow, every metadata tag, every input value, every chart axis label, every button label. Always uppercase, always with wide letter-spacing.
- **Jost / DM Sans** is the body face: paragraphs, list items, card descriptions, table cells.

### 4.2 Type scale

```css
:root {
  --text-2xs:  0.5rem;     /*  8px — mono micro-labels (NPD stage headers, mini-pill copy) */
  --text-xs:   0.625rem;   /* 10px — eyebrow labels, metadata, slider min/max */
  --text-xs-2: 0.6875rem;  /* 11px — small body, footnotes */
  --text-sm:   0.75rem;    /* 12px — body small, card descriptions */
  --text-sm-2: 0.8125rem;  /* 13px — body default, input values */
  --text-base: 0.875rem;   /* 14px — long-form body */
  --text-md:   0.9375rem;  /* 15px — emphasis body */
  --text-lg:   1rem;       /* 16px — card titles (rare) */
  --text-xl:   1.125rem;   /* 18px — small actor headings */
  --text-2xl:  1.375rem;   /* 22px — stat number small */
  --text-3xl:  1.75rem;    /* 28px — actor card title */
  --text-4xl:  2rem;       /* 32px — pattern card display title */
  --text-5xl:  2.125rem;   /* 34px — flywheel hub headline */
  --text-6xl:  2.625rem;   /* 42px — section h2 small clamp */
  --text-7xl:  3.375rem;   /* 54px — section h2 large clamp */
  --text-8xl:  6rem;       /* 96px — hero h1 max clamp */

  /* Line heights */
  --leading-display:  0.9;     /* Hero h1 */
  --leading-tight:    0.95;    /* Section h2 */
  --leading-snug:     1.15;    /* Stat values */
  --leading-normal:   1.5;
  --leading-readable: 1.65;    /* Card body */
  --leading-airy:     1.75;    /* Section intro paragraph */
  --leading-loose:    1.8;     /* Long-form */

  /* Letter spacing */
  --tracking-tight:   -0.02em;
  --tracking-normal:   0;
  --tracking-wide:     0.05em;
  --tracking-mono-1:   1px;    /* DM Mono short labels */
  --tracking-mono-2:   1.5px;
  --tracking-mono-3:   2px;    /* DM Mono eyebrow */
  --tracking-mono-4:   3px;    /* DM Mono section eyebrow */
}
```

### 4.3 Type roles (canonical bindings)

| Role | Family | Size | Weight | Case | Tracking | Line-height | Color |
|---|---|---|---|---|---|---|---|
| Hero h1 | display | `clamp(58px, 8.5vw, 96px)` | 400 (Bebas only ships 400) | Sentence | normal | 0.9 | `--color-text-primary` |
| Section h2 (large) | display | `clamp(34px, 5vw, 54px)` | 400 | Sentence | normal | 0.95 | `--color-text-primary` |
| Section h2 (small) | display | `clamp(28px, 4vw, 42px)` | 400 | Sentence | normal | 0.95 | `--color-text-primary` |
| Pattern title | display | 32px | 400 | Sentence | normal | 1 | `var(--accent)` |
| Actor card title | display | 28px | 800 (DM Sans fallback OK) | Sentence | normal | 1.1 | `--color-light-card-text` |
| Stat value (large) | display | 22–34px | 400 | — | normal | 1 | `--color-text-primary` or `var(--accent)` |
| Eyebrow / overline | mono | 10px (8–12 OK) | 500 | UPPERCASE | 2–3px | 1.4 | `var(--accent)` |
| Pill / metadata | mono | 8–10px | 500 | UPPERCASE | 1–2px | 1.3 | `var(--accent)` |
| Card title | body | 13–15px | 700 | Sentence | normal | 1.35 | `--color-text-primary` or `var(--accent)` |
| Body lead (section intro) | body | 14–15px | 400 | Sentence | normal | 1.8 | `--color-text-body` |
| Body default | body | 12–13px | 400 | Sentence | normal | 1.65–1.75 | `--color-text-body` |
| Body small | body | 11px | 400 | Sentence | normal | 1.65 | `--color-text-body` |
| Footnote | body | 9–10px | 400 | Sentence | normal | 1.5 | `--color-text-muted` |
| Button label | mono | 11px | 500 | Sentence (lowercased OK) | normal | 1 | varies (active = accent, idle = `--color-text-secondary`) |
| Input value | mono | 13px | 400 | — | normal | 1 | `--color-text-primary` |
| Table header | mono | 10px | 500 | UPPERCASE | 2px | 1.4 | `--color-text-secondary` |
| Table cell | body | 11–13px | 400 | — | normal | 1.6 | `--color-text-body` |

### 4.4 Type rules

- **All labels (eyebrow, metadata pill, table header, mono badge, button text) must use DM Mono in `UPPERCASE` with letter-spacing ≥ 1px.** No exceptions.
- **Bebas Neue is never lower-cased.** It is a display face used for emphasis blocks (titles, stats); it does not appear inside paragraph text.
- **Body copy is set in Jost between 11–15px** with line-height between 1.65 and 1.8. Bigger than 16px in body copy is forbidden.
- Strong emphasis inside body copy: `<strong>` with `color: var(--color-text-primary)` — never bold + accent together; pick one.

---

## 5. Spacing

```css
:root {
  --space-0-5: 2px;   /* Micro borders, grid gap on stage trackers */
  --space-1:   4px;
  --space-1-5: 6px;
  --space-2:   8px;
  --space-3:  12px;
  --space-4:  14px;
  --space-5:  16px;
  --space-6:  18px;
  --space-7:  20px;
  --space-8:  22px;
  --space-9:  24px;
  --space-10: 26px;
  --space-12: 32px;
  --space-14: 36px;   /* Section vertical separator */
  --space-16: 40px;
  --space-20: 48px;
  --space-24: 60px;
  --space-28: 72px;   /* Hero top padding */
  --space-32: 80px;
}
```

- **Card internal padding:** `14–22px` vertical, `14–28px` horizontal. Default: `16px 18px` or `18px 22px`.
- **Card-to-card gap inside a grid:** `10–14px`.
- **Eyebrow → h2 spacing:** `6–10px`.
- **h2 → body lead spacing:** `10–14px`.
- **Section-to-section spacing:** a 1px hairline at `--hairline-on-dark`, with `36px` margin top and bottom.
- **Container side padding:** `24px` (matches General's `padding:"0 24px 100px"`).
- **Container max-width:** `980px` for content; specific widgets cap at `640px` (funnel), `780px` (hero intro), `860px` (flywheel).

---

## 6. Layout

### 6.1 App shell

```
┌──────────────────────────────────────────────────────────┐
│  STICKY NAV — translucent navy + accent-border-bottom    │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  ┌──── main container, max-width 980px, padding 24px ──┐ │
│  │                                                     │ │
│  │  Eyebrow label                                      │ │
│  │  Section h2                                         │ │
│  │  Body lead                                          │ │
│  │  Cards / charts / tables                            │ │
│  │                                                     │ │
│  │  ── hairline ──                                     │ │
│  │                                                     │ │
│  │  Next section…                                      │ │
│  │                                                     │ │
│  └─────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────┘
```

### 6.2 Common grid templates

```css
.grid-2     { display: grid; grid-template-columns: repeat(2, 1fr);  gap: 14px; }
.grid-3     { display: grid; grid-template-columns: repeat(3, 1fr);  gap: 14px; }
.grid-4     { display: grid; grid-template-columns: repeat(4, 1fr);  gap: 10px; }
.grid-7-bar { display: grid; grid-template-columns: repeat(7, 1fr);  gap: 3px;  } /* NPD stage tracker */

.grid-actors {
  display: grid;
  grid-template-columns: 280px minmax(320px, 400px) 280px;
  justify-content: space-between;
  gap: 26px;
  align-items: center;
  min-height: 430px;
}

@media (max-width: 768px) {
  .grid-2, .grid-3, .grid-4 { grid-template-columns: 1fr; }
  .grid-actors { grid-template-columns: 1fr; min-height: 0; }
}
```

---

## 7. Surface Depth Hierarchy

Cards stack on a five-tier depth ladder. **Pick the depth based on context, not aesthetic.**

| Tier | Token | Hex | Use |
|---|---|---|---|
| 1 (page) | `--color-bg-page` | `#0D1E35` | Body background |
| 2 (app body) | `--color-bg-app` | `#0A1828` | Container under sticky nav |
| 3 (card) | `--color-bg-card` | `#002A45` | Default content card |
| 4 (deep card) | `--color-bg-card-deep` | `#001829` | Section opener card with deep accent |
| 5 (input / mid) | `--color-bg-card-mid` / `--color-bg-input` | `#002040` / `#001E35` | Stat tile, input field |

**Rule:** never place a tier-3 card on a tier-2 surface without a 1px hairline border. Tier-4 cards may sit on tier-3 cards.

---

## 8. Borders, Radii, Shadows

### 8.1 Border radius

```css
:root {
  --radius-1:  2px;   /* DEFAULT — cards, buttons, pills, badges, table cells, inputs */
  --radius-2:  3px;   /* Slightly enlarged cards, navigation pills */
  --radius-3:  4px;   /* Flywheel hub */
  --radius-4:  6px;
  --radius-5:  8px;   /* Outer wrapper of actor flow diagram (light surface) */
  --radius-6: 12px;   /* Panel component */
  --radius-7: 16px;   /* Inner mini-tile inside actor card */
  --radius-8: 18px;   /* Sub-section pill inside diagram */
  --radius-9: 28px;   /* Gradient actor side cards */
  --radius-10:34px;   /* Center actor card (white) */
  --radius-pill: 999px;
}
```

- **Default radius for any card, button, input, badge, table is `--radius-1` (2px).** This is the heartbeat of the system — sharp, financial, precise.
- **Larger radii (8–34px) only appear inside the actor flow diagram (§9.18) and the circular flywheel (§9.19).** They make those diagrams visually distinct from content cards.
- **The Panel component (§9.20) uses `--radius-6` (12px)** as it represents a separate "module" inside a section.
- Avatars / dots / progress dots: `border-radius: 50%`.

### 8.2 Border styles

| Use | Value |
|---|---|
| Card hairline | `1px solid var(--color-border-card)` |
| Card hairline strong | `1px solid var(--color-border-strong)` |
| Tinted card border | `1px solid {accent}25–{accent}40` (20–30% opacity) |
| Accent top border | `2px solid var(--accent)` |
| Accent left border (risk / pattern card) | `3px solid var(--accent)` |
| Emphasis outline (white actor card) | `1.5px solid rgba(14,22,51,.24)` |

### 8.3 Box shadows

```css
:root {
  --shadow-card:   0 6px 18px rgba(0, 0, 0, 0.16);   /* Flywheel nodes */
  --shadow-hub:    0 8px 24px rgba(0, 0, 0, 0.25);   /* Flywheel center */
  --shadow-actor:  0 12px 30px rgba(0, 0, 0, 0.16);  /* Gradient actor side cards */
  --shadow-light:  0 8px 22px rgba(0, 0, 0, 0.08);   /* White actor center card */
  --shadow-subtle: 0 2px 20px rgba(0, 0, 0, 0.18);
}
```

- **Plain content cards do not use box-shadow.** Shadows are reserved for the flow diagram and the flywheel.
- Never use colored / tinted shadows.

---

## 9. Components (Canonical)

For every component below, prefer the class names in `assets/css/tipolis-design-system.css`. If you need an inline variant, follow the exact token values.

### 9.1 Section header (eyebrow + h2 + lead)

```html
<header class="t-section-header">
  <span class="t-eyebrow">Strategic Framework 05</span>
  <h2 class="t-h2">New Product Development — Where Is Each Initiative Today?</h2>
  <p class="t-lead">The NPD framework maps the journey from idea to launch across seven stages…</p>
</header>
```

```css
.t-eyebrow {
  display: inline-block;
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  font-weight: 500;
  letter-spacing: var(--tracking-mono-4);
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 10px;
}
.t-h2 {
  font-family: var(--font-display);
  font-size: clamp(34px, 5vw, 54px);
  line-height: var(--leading-tight);
  margin-bottom: 14px;
  color: var(--color-text-primary);
}
.t-h2--sm { font-size: clamp(28px, 4vw, 42px); }
.t-lead {
  font-family: var(--font-body);
  font-size: 14px;
  line-height: var(--leading-loose);
  color: var(--color-text-body);
  margin-bottom: 36px;
  max-width: 780px;
}
```

### 9.2 IBadge — pill with colored dot

A small inline pill that tags a section or status.

```html
<span class="t-ibadge" style="--accent: var(--accent-treasury)">
  <span class="t-ibadge__dot"></span>
  <span class="t-ibadge__label">Strategic Capital Proposal</span>
</span>
```

```css
.t-ibadge {
  display: inline-flex;
  align-items: center;
  gap: 7px;
  padding: 5px 13px;
  margin-bottom: 22px;
  background: color-mix(in srgb, var(--accent) 8%, transparent);
  border: 1px solid color-mix(in srgb, var(--accent) 25%, transparent);
  border-radius: var(--radius-1);
}
.t-ibadge__dot {
  width: 6px; height: 6px; border-radius: 50%;
  background: var(--accent);
}
.t-ibadge__label {
  font-family: var(--font-mono);
  font-size: 10px;
  letter-spacing: var(--tracking-mono-3);
  text-transform: uppercase;
  color: var(--accent);
}
```

### 9.3 Risk Card — colored left border, indicator dot

Used for risk lists, status entries, or any rank-style content where each row carries a severity color.

```html
<article class="t-risk" style="--accent: var(--accent-warning)">
  <span class="t-risk__indicator"></span>
  <div class="t-risk__body">
    <h4 class="t-risk__title">Regulatory uncertainty</h4>
    <span class="t-risk__badge">High</span>
    <p class="t-risk__text">MiCA compliance pathway is well-defined but not yet exercised at scale.</p>
  </div>
</article>
```

```css
.t-risk {
  display: flex;
  gap: 14px;
  padding: 16px 18px;
  background: var(--color-bg-card-surface);   /* #003050 */
  border-left: 3px solid var(--accent);
  border-radius: var(--radius-1);
  margin-bottom: 10px;
}
.t-risk__indicator {
  width: 9px; height: 9px; border-radius: 50%;
  background: var(--accent); margin-top: 4px; flex-shrink: 0;
}
.t-risk__title { font-size: 14px; font-weight: 600; margin-bottom: 3px; color: var(--color-text-primary); }
.t-risk__badge {
  display: inline-block;
  font-family: var(--font-mono); font-size: 9px;
  letter-spacing: 1px; text-transform: uppercase;
  padding: 2px 8px; border-radius: var(--radius-1);
  background: color-mix(in srgb, var(--accent) 10%, transparent);
  color: var(--accent);
  margin-bottom: 5px;
}
.t-risk__text { font-size: 12px; color: var(--color-text-secondary); line-height: var(--leading-readable); }
```

### 9.4 Feature Card (top-accent)

The most common content card. A 2px colored top border, mono pre-label, body description.

```html
<article class="t-feature" style="--accent: var(--accent-bonds)">
  <span class="t-feature__pre">01 · STRC Treasury</span>
  <p class="t-feature__body">Tipolis moves idle cash into STRC and immediately earns more than T-Bills…</p>
</article>
```

```css
.t-feature {
  background: var(--color-bg-card-mid);     /* #002040 */
  border: 1px solid rgba(255,255,255,.06);
  border-top: 2px solid var(--accent);
  border-radius: var(--radius-2);
  padding: 14px 18px;
}
.t-feature__pre {
  display: block;
  font-family: var(--font-mono);
  font-size: 9px;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 8px;
}
.t-feature__body { font-size: 12px; line-height: 1.7; color: var(--color-text-body); }
```

### 9.5 Tinted Card (accent background)

Used for callouts and grouped content blocks. Accent-tinted background and border, no left bar.

```css
.t-tinted {
  background: color-mix(in srgb, var(--accent)  6%, transparent);
  border: 1px solid color-mix(in srgb, var(--accent) 20%, transparent);
  border-radius: var(--radius-1);
  padding: 16px 18px;
}
.t-tinted__eyebrow {
  font-family: var(--font-mono);
  font-size: 9px; letter-spacing: 2px; text-transform: uppercase;
  color: var(--accent); margin-bottom: 8px;
}
.t-tinted__body { font-size: 13px; color: var(--color-text-body); line-height: 1.8; }
```

### 9.6 KPI / Stat Block

A tile with a stat number and a label below. Use Bebas Neue for the number.

```html
<div class="t-stat" style="--accent: var(--accent-treasury)">
  <span class="t-stat__label">Gross Revenue</span>
  <span class="t-stat__value">$7.8M</span>
  <span class="t-stat__sub">2023 Treasury · YoY +23%</span>
</div>
```

```css
.t-stat {
  background: var(--color-bg-card-mid);
  border: 1px solid rgba(255,255,255,.06);
  border-radius: var(--radius-1);
  padding: 12px 14px;
}
.t-stat__label {
  display: block;
  font-family: var(--font-mono); font-size: 9px;
  letter-spacing: 1px; text-transform: uppercase;
  color: var(--color-text-secondary); margin-bottom: 4px;
}
.t-stat__value {
  display: block;
  font-family: var(--font-display);
  font-size: 22px; line-height: 1;
  color: var(--accent); margin-bottom: 2px;
}
.t-stat__sub { font-size: 9px; color: var(--color-text-muted); }
```

Stat grid:

```css
.t-stat-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; }
@media (max-width: 768px) { .t-stat-grid { grid-template-columns: repeat(2, 1fr); } }
```

### 9.7 Callout Box

Used for "key takeaway" / quote-style emphasis at the end of a section.

```html
<aside class="t-callout" style="--accent: var(--accent-digital)">
  <strong>Key takeaway:</strong> The NPD stages are a deliberate sequence, not a coincidence.
</aside>
```

```css
.t-callout {
  background: color-mix(in srgb, var(--accent) 6%, transparent);
  border: 1px solid color-mix(in srgb, var(--accent) 20%, transparent);
  border-radius: var(--radius-1);
  padding: 12px 16px;
  font-size: 12px;
  line-height: 1.75;
  color: var(--color-text-body);
}
.t-callout strong { color: var(--accent); }
```

### 9.8 Loop / Cycle Box (pair)

Used to display reinforcing loops (Loop A / Loop B). Always rendered as a 2-column grid where each box carries its own accent.

```html
<div class="grid-2">
  <div class="t-loop" style="--accent: var(--accent-digital)">
    <span class="t-loop__title">🟡 Loop A — Treasury to Digital Money</span>
    <p class="t-loop__body">…</p>
  </div>
  <div class="t-loop" style="--accent: var(--accent-bonds)">
    <span class="t-loop__title">🔵 Loop B — Digital Money to City Bonds</span>
    <p class="t-loop__body">…</p>
  </div>
</div>
```

```css
.t-loop {
  background: color-mix(in srgb, var(--accent) 6%, transparent);
  border: 1px solid color-mix(in srgb, var(--accent) 20%, transparent);
  border-radius: var(--radius-2);
  padding: 16px 18px;
}
.t-loop__title {
  display: block;
  font-family: var(--font-mono); font-size: 9px;
  letter-spacing: 2px; text-transform: uppercase;
  color: var(--accent); margin-bottom: 8px;
}
.t-loop__body { font-size: 12px; line-height: 1.75; color: var(--color-text-body); }
.t-loop__body strong { color: var(--color-text-primary); }
```

### 9.9 Pattern Card (left-border 3px, hero variant)

Reserved for "primary patterns" — the single dominant block at the top of a pattern grid.

```css
.t-pattern {
  background: linear-gradient(135deg,
    color-mix(in srgb, var(--accent) 10%, transparent),
    color-mix(in srgb, var(--accent)  3%, transparent));
  border: 2px solid var(--accent);
  border-radius: var(--radius-2);
  padding: 24px 28px;
  margin-bottom: 12px;
}
.t-pattern__pre {
  font-family: var(--font-mono); font-size: 8px;
  letter-spacing: 3px; text-transform: uppercase;
  color: var(--accent); margin-bottom: 4px;
}
.t-pattern__star {
  font-family: var(--font-mono); font-size: 11px;
  letter-spacing: 1px; color: var(--accent); margin-bottom: 4px;
}
.t-pattern__title {
  font-family: var(--font-display); font-size: 32px;
  line-height: 1; color: var(--accent); margin-bottom: 12px;
}
.t-pattern__body { font-size: 13px; color: var(--color-text-body); line-height: 1.75; }
```

### 9.10 Tables / Comparison Grid

Two patterns are valid: a **CSS grid table** (preferred — more flexible on dark backgrounds) and a true `<table>`.

```css
.t-table {
  width: 100%;
  border-collapse: collapse;
  font-family: var(--font-body);
}
.t-table thead th {
  font-family: var(--font-mono);
  font-size: 10px;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: var(--color-text-secondary);
  text-align: left;
  padding: 12px 14px;
  border-bottom: 1px solid var(--color-border-card);
}
.t-table tbody td {
  font-size: 12px;
  color: var(--color-text-body);
  padding: 12px 14px;
  border-bottom: 1px solid var(--hairline-on-dark);
  vertical-align: middle;
}
.t-table tr.is-highlight td {
  background: color-mix(in srgb, var(--accent) 10%, transparent);
  color: var(--color-text-primary);
  font-weight: 600;
}
```

### 9.11 Buttons & Page Switcher pills

Two button styles only:

**Primary action button** (rare — only used in CTA blocks):

```css
.t-btn-primary {
  font-family: var(--font-mono); font-size: 11px; font-weight: 500;
  letter-spacing: var(--tracking-wide); text-transform: uppercase;
  color: var(--color-navy-900);
  background: var(--accent);
  border: none; border-radius: var(--radius-1);
  padding: 10px 22px; cursor: pointer;
  transition: background var(--transition-fast);
}
.t-btn-primary:hover { filter: brightness(1.1); }
```

**Pill switcher button** (used in the sticky nav and in any tabbed panel):

```css
.t-pill {
  display: inline-flex; align-items: center;
  padding: 5px 13px;
  font-family: var(--font-mono);
  font-size: 11px;
  white-space: nowrap;
  border: 1px solid var(--color-text-secondary);
  background: transparent;
  color: var(--color-text-secondary);
  border-radius: var(--radius-2);
  cursor: pointer;
  transition: all 200ms ease;
}
.t-pill[aria-pressed="true"],
.t-pill.is-active {
  border-color: var(--accent);
  background: color-mix(in srgb, var(--accent) 7%, transparent);
  color: var(--accent);
}
```

### 9.12 Form inputs (text / number)

```css
.t-input {
  width: 100%;
  background: var(--color-bg-input);
  border: 1px solid var(--hairline-on-dark-3);
  border-radius: var(--radius-1);
  padding: 8px 10px;
  font-family: var(--font-mono); font-size: 13px;
  color: var(--color-text-primary);
  outline: none;
  transition: border-color var(--transition-fast);
}
.t-input:focus-visible { border-color: var(--accent); }

.t-input-label {
  display: block;
  font-family: var(--font-mono); font-size: 9px;
  letter-spacing: 1.5px; text-transform: uppercase;
  color: var(--color-text-secondary);
  margin-bottom: 5px;
}
```

### 9.13 Range slider

```html
<div class="t-range">
  <div class="t-range__row">
    <span class="t-input-label">Gross Revenue % of AUM</span>
    <span class="t-range__readout">1.00%</span>
  </div>
  <input type="range" min="0.5" max="2.5" step="0.05" value="1.0" class="t-range__input">
  <div class="t-range__bounds"><span>0.5%</span><span>← drag</span><span>2.5%</span></div>
</div>
```

```css
.t-range__row {
  display: flex; justify-content: space-between; align-items: baseline;
  margin-bottom: 6px;
}
.t-range__readout {
  font-family: var(--font-mono); font-size: 13px; font-weight: 700;
  color: var(--accent);
}
.t-range__input {
  width: 100%; accent-color: var(--accent); cursor: pointer;
}
.t-range__bounds {
  display: flex; justify-content: space-between;
  font-family: var(--font-mono); font-size: 9px;
  color: var(--color-text-on-dark-fainter);
  margin-top: 3px;
}
```

### 9.14 Progress bar (allocation visualization)

```css
.t-progress {
  background: rgba(255,255,255,.06);
  border-radius: var(--radius-1);
  height: 8px; overflow: hidden;
}
.t-progress__fill {
  height: 100%; border-radius: var(--radius-1);
  background: var(--accent);
  transition: width 400ms cubic-bezier(.16, 1, .3, 1);
}
```

Color reaction is allowed: when `value > 60%` use `--accent-strategic`, `30–60%` use `--accent-digital`, `< 30%` use `--accent-treasury`.

### 9.15 Animated Number

Display-face number that counts up on viewport entry. Implementation pattern (vanilla JS):

```js
function animNum(el, target, dur=1500){
  const t = parseFloat(target), s = performance.now();
  const step = n => {
    const p = Math.min((n - s) / dur, 1);
    const e = 1 - Math.pow(1 - p, 3);  // ease-out cubic
    el.textContent = (e * t).toFixed(Number.isInteger(t) ? 0 : 1);
    if (p < 1) requestAnimationFrame(step);
  };
  requestAnimationFrame(step);
}
```

- Trigger only when the element enters the viewport (use `IntersectionObserver`).
- Default duration: `1500ms`. Easing: ease-out cubic.
- Always render the final value in the DOM as fallback (no JS = static number).

### 9.16 NPD Stage Tracker

A 7-column bar chart used for staged progress. Each column is a 28px-tall block; the column at the current stage is outlined `2px solid var(--accent)` with a small "NOW" mono label inside.

```css
.t-stage-track { display: grid; grid-template-columns: repeat(7, 1fr); gap: 3px; }
.t-stage-track__cell {
  height: 28px; border-radius: var(--radius-1);
  display: flex; align-items: center; justify-content: center;
  position: relative; transition: all 200ms;
}
.t-stage-track__cell--inactive {
  background: rgba(255,255,255,.04);
  border: 1px solid rgba(255,255,255,.06);
}
.t-stage-track__cell--active {
  background: color-mix(in srgb, var(--accent) 60%, transparent);
  border: 1px solid rgba(255,255,255,.06);
}
.t-stage-track__cell--current {
  background: var(--accent);
  border: 2px solid var(--accent);
}
.t-stage-track__cell--current::after {
  content: 'NOW';
  font-family: var(--font-mono); font-size: 8px; font-weight: 700;
  letter-spacing: .5px; color: var(--color-navy-800);
}
```

### 9.17 Timeline (roadmap)

Used for milestone sequences (Months 1–3 → 4–9 → 10+).

```css
.t-timeline {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  position: relative;
}
.t-timeline::before {
  content: ''; position: absolute; top: 20px; left: 0; right: 0;
  height: 1px; background: var(--color-border-card);
}
.t-timeline__step { padding-top: 40px; padding-right: 32px; position: relative; }
.t-timeline__step::before {
  content: ''; position: absolute; top: 14px; left: 0;
  width: 12px; height: 12px; border-radius: 50%;
  background: var(--accent);
}
.t-timeline__period {
  font-family: var(--font-mono); font-size: 10px;
  letter-spacing: 2px; text-transform: uppercase;
  color: var(--color-text-secondary); margin-bottom: 12px;
}
.t-timeline__title {
  font-family: var(--font-display); font-size: 20px; line-height: 1.1;
  margin-bottom: 14px; color: var(--color-text-primary);
}
.t-timeline__items { list-style: none; display: flex; flex-direction: column; gap: 8px; }
.t-timeline__item {
  font-size: 12px; line-height: 1.65;
  color: var(--color-text-body);
  padding-left: 14px; position: relative;
}
.t-timeline__item::before {
  content: '▸'; position: absolute; left: 0; color: var(--accent);
}
```

### 9.18 Actor Flow Diagram (3-column)

Used to show capital / value flow between three actors (e.g., *Investors → Digital Bank → Users*). It is the **only place** in the design system where a white surface is permitted.

Structure:

```
┌──────── light wrapper (#F3F6FA, radius 8) ──────────┐
│  [ blue gradient ]   [ WHITE actor card  ]   [ blue gradient ]
│  [ left actor    ]   [ central operator ]   [ right actor    ]
│  radius-9 (28px)    radius-10 (34px)        radius-9 (28px)
└─────────────────────────────────────────────────────┘
+ 4 numbered SVG connectors (01, 02, 03, 04) labeled in dark text
+ optional row of 4 micro-tiles below describing the flow
```

```css
.t-flow {
  background: var(--color-light-bg);          /* #F3F6FA */
  border-radius: var(--radius-5);             /* 8px */
  padding: 26px 20px 18px;
  border: 1px solid var(--hairline-on-dark-2);
}
.t-flow__row { /* extends .grid-actors */ }

.t-flow__actor {
  background: var(--gradient-actor);
  color: var(--color-text-on-dark);
  border-radius: var(--radius-9);             /* 28px */
  padding: 28px 18px;
  min-height: 300px;
  box-shadow: var(--shadow-actor);
  text-align: center;
}
.t-flow__center {
  background: var(--color-light-card);
  color: var(--color-light-card-text);
  border: 1.5px solid rgba(14,22,51,.24);
  border-radius: var(--radius-10);            /* 34px */
  padding: 24px 18px;
  min-height: 356px;
  box-shadow: var(--shadow-light);
}
.t-flow__center-eyebrow {
  font-family: var(--font-mono); font-size: 12px;
  letter-spacing: 1.8px; text-transform: uppercase;
  color: var(--color-light-card-text); margin-bottom: 10px;
}
.t-flow__center-title {
  font-family: var(--font-display); font-size: 28px;
  line-height: 1.1; color: var(--color-light-card-text);
}
.t-flow__mini {
  border-radius: var(--radius-7);             /* 16–18px */
  padding: 14px 12px;
  text-align: center;
  background: color-mix(in srgb, var(--accent) 8%, transparent);
  border: 1px solid color-mix(in srgb, var(--accent) 22%, transparent);
}
.t-flow__mini-title { font-size: 12px; font-weight: 700; color: var(--accent); margin-bottom: 6px; }
.t-flow__mini-body  { font-size: 11px; color: #5A6A7A; line-height: 1.5; }
```

The four connecting numbered labels (01–04) sit in a relatively-positioned SVG layer between the columns.

### 9.19 Circular Flywheel

A circular diagram with **one central hub** + **six outer nodes** + **SVG arrows tracing the rotation**. Total canvas 760×860px on desktop; collapses to a stacked list below 768px.

```css
.t-flywheel {
  position: relative;
  width: 100%; max-width: 860px; height: 760px;
  margin: 0 auto;
}
.t-flywheel__hub {
  position: absolute; left: 50%; top: 50%;
  transform: translate(-50%, -50%);
  width: 250px; min-height: 170px;
  background: var(--gradient-hub);
  border: 1px solid var(--hairline-on-dark-3);
  border-radius: var(--radius-3);             /* 4px */
  box-shadow: var(--shadow-hub);
  padding: 18px 22px;
  text-align: center;
}
.t-flywheel__hub-title {
  font-family: var(--font-display); font-size: 34px;
  line-height: 1; color: var(--color-text-primary); margin-bottom: 8px;
}
.t-flywheel__node {
  position: absolute;
  width: 230px; min-height: 112px;
  background: var(--color-bg-card-deep);      /* #002A45 */
  border: 1px solid color-mix(in srgb, var(--node-accent) 25%, transparent);
  border-top: 3px solid var(--node-accent);
  border-radius: var(--radius-2);
  box-shadow: var(--shadow-card);
  padding: 16px 16px 15px;
  text-align: center;
}
.t-flywheel__node-title {
  font-size: 12px; font-weight: 700;
  color: var(--node-accent); line-height: 1.35; margin-bottom: 8px;
}
.t-flywheel__node-body { font-size: 11px; color: var(--color-text-body); line-height: 1.65; }
```

Each node sets `--node-accent` inline (`#FF6B35`, `#F5A623`, `#3B82F6`, etc.). The six arrows are absolutely-positioned SVG `<path>` arrows, 3px stroke, rounded caps, 95% opacity.

### 9.20 Market Funnel (TAM → SAM → SOM)

Stacked trapezoids rendered in SVG, top trapezoid widest, bottom narrowest. Each step has a top width and a bottom width (in % units multiplied by 6 for the 600-unit viewBox).

```html
<figure class="t-funnel" style="--accent: var(--accent-peer)">
  <figcaption class="t-funnel__title">Market Funnel — TAM → SAM → SOM</figcaption>
  <!-- stacked SVG trapezoids per step (see general/index.html lines 153–192) -->
</figure>
```

```css
.t-funnel { margin: 36px auto; max-width: 640px; }
.t-funnel__title {
  font-family: var(--font-mono); font-size: 10px;
  letter-spacing: 2px; text-transform: uppercase;
  color: var(--accent); margin-bottom: 24px; text-align: center;
}
```

Each trapezoid fill is `linearGradient` from accent at 35% opacity to 15% opacity (180deg). Stroke is the accent at 60% opacity. Value labels: DM Mono, 13px, weight 700, white. Step labels: DM Mono, 9px, accent, 2px tracking, uppercase, centered below the value.

### 9.21 Section Divider

```css
.t-divider {
  height: 1px;
  background: var(--hairline-on-dark);   /* rgba(255,255,255,.05) */
  margin: 36px 0;
  border: none;
}
```

Use a `<hr class="t-divider">` (or a styled `<div>`) between every section transition. Never style `<hr>` differently elsewhere.

---

## 10. Page Header / Hero

Hero block sits **above** the first section. Tall, eyebrow + display title with optional accent-colored sub-clause, body lead, optional CTA cards.

```css
.t-hero {
  padding: 72px 0 48px;
  border-bottom: 1px solid var(--hairline-on-dark);
}
.t-hero__overline {
  font-family: var(--font-mono); font-size: 10px;
  letter-spacing: 3px; text-transform: uppercase;
  color: var(--accent); margin-bottom: 14px;
}
.t-hero__title {
  font-family: var(--font-display);
  font-size: clamp(58px, 8.5vw, 96px);
  line-height: .9;
  color: var(--color-text-primary);
  margin-bottom: 22px;
}
.t-hero__title span { color: var(--accent); }   /* accent-colored sub-clause */
.t-hero__lead {
  max-width: 780px;
  font-size: 15px; line-height: 1.8;
  color: var(--color-text-body);
  margin-bottom: 40px;
}
```

The page header on a sub-route (`strc-treasury/`, `digital-money/`, etc.) **must** open with this hero block. Plain `<h1>` outside of `.t-hero` is forbidden.

---

## 11. Navigation

Sticky top bar with logo + horizontal pill switcher.

```css
.t-nav {
  position: sticky; top: 0; z-index: 100;
  background: rgba(0, 30, 48, 0.97);
  backdrop-filter: blur(18px);
  -webkit-backdrop-filter: blur(18px);
  border-bottom: 1px solid color-mix(in srgb, var(--accent) 14%, transparent);
  display: flex; align-items: center; gap: 6px;
  padding: 12px 24px;
  overflow-x: auto;
}
.t-nav__brand { display: flex; align-items: center; gap: 8px; margin-right: 14px; white-space: nowrap; }
.t-nav__brand img { height: 30px; width: auto; }
.t-nav__brand span {
  font-family: var(--font-body);   /* DM Sans bold reads better than Jost here */
  font-size: 18px; font-weight: 700;
  color: var(--color-text-primary);
  letter-spacing: .5px;
}
.t-nav__items { display: flex; gap: 6px; }
.t-nav__item { /* extends .t-pill */ }
```

The active pill carries the page accent; inactive pills use `--color-text-secondary` (`#7A8A9E`) for border and text.

---

## 12. Logo

The Tipolis word-mark is a small image (typically 30px high) followed by the wordmark "Tipolis" in **DM Sans 18/700**. Place it left-aligned in the sticky nav, centered in hero blocks of marketing slides.

- Clear space: 16px on all sides.
- Minimum size: 24px image height.
- Colors: always white on dark. Never tint with the accent.

---

## 13. Charts & SVG

All custom charts in the repo follow these rules:

1. **Render in SVG** — never canvas-only, never raster. SVG is accessible, themable via tokens, and scales.
2. **Stroke width:** `1.5px` for default lines; `2–3px` for emphasized lines (axis baselines, current value).
3. **Fills:** linear gradients from the active accent at 35% opacity (top) to 15% (bottom). Solid fills are forbidden for areas; only stroke lines may be solid.
4. **Labels inside charts:** DM Mono, 9–13px, uppercase with `letter-spacing: 2px`. Color = active accent or `--color-text-body`.
5. **Axis colors:** `rgba(255,255,255,.12)`.
6. **No 3D, no donut percentages, no shadow under bars.**
7. **Bar / line entry animation:** `600ms ease-out` from bottom or left. Numbers count up via the `animNum` helper.

---

## 14. Iconography

Two icon vocabularies coexist:

### 14.1 Emoji icons (used in General)

Allowed and encouraged for executive summary cards, actor headers, and loop titles where a small visual cue replaces an SVG icon. Standard set:

| Emoji | Use |
|---|---|
| 💰 | Treasury / capital |
| 🏦 | Bank / financial product |
| 🏗️ | Infrastructure / development |
| 👤 / 🏢 | Resident / business actor |
| 📊 | Data / report |
| 🪙 | Token / digital money |
| 🏙️ | City / jurisdiction |
| 📡 | Network / signal |
| 🟡 / 🔵 / 🟢 / 🟠 / 🟣 | Color-coded loop or section marker |
| ▸ / → / ★ | Inline list bullets and emphasis (rendered as text) |

Display rule: emoji size 18–24px inside cards, 24–38px for hero-style accents. Always paired with a DM Mono label.

### 14.2 Stroke SVG icons (optional)

When an emoji is insufficient, use a Lucide-style outline icon at `stroke-width: 1.5`, `fill: none`, `stroke: currentColor`. Sizes: 16 / 20 / 24 / 40 / 48 px.

Never combine emoji and SVG icons in the same card.

---

## 15. Motion

```css
:root {
  --transition-fast:   200ms ease;
  --transition-normal: 400ms cubic-bezier(.16, 1, .3, 1);
  --transition-slow:   800ms ease;
}
```

Allowed motion:

| Trigger | Effect | Duration | Easing |
|---|---|---|---|
| Hover on pill / button / link | color, border-color, background | 200ms | ease |
| Slider drag → progress fill width | width | 400ms | cubic-bezier(.16, 1, .3, 1) |
| Section enter viewport | opacity 0→1, translateY 16→0 | 800ms | ease |
| Counter / stat number reaches viewport | value 0→target | 1500ms | ease-out cubic |
| Stage tracker cell becomes "current" | background, border | 200ms | ease |
| Pulse (loading) | `@keyframes pulse` (opacity 1↔.4) | 1500ms | ease, infinite |

Forbidden: bounce, elastic, rotate, scale, swing, spring, color-cycling backgrounds.

`@media (prefers-reduced-motion: reduce) { * { animation: none !important; transition: none !important; } }`

---

## 16. Page Templates

### 16.1 Topic page (default for every sub-directory)

```
<head> — required font + CSS links (§2)
<body class="page--[topic-slug]">
  <nav class="t-nav"> Logo + pill switcher </nav>
  <main class="t-main" style="max-width:980px; margin:0 auto; padding:0 24px 100px">

    <header class="t-hero">
      <span class="t-hero__overline">Topic eyebrow</span>
      <h1 class="t-hero__title">Headline. <span>Accent sub-clause.</span></h1>
      <p class="t-hero__lead">Body lead…</p>
    </header>

    <section>
      <span class="t-eyebrow">Strategic Framework 01</span>
      <h2 class="t-h2">Section title</h2>
      <p class="t-lead">Body lead…</p>

      <!-- content blocks: feature cards / loop boxes / stat grid / table -->
    </section>

    <hr class="t-divider">

    <section>...</section>
    <hr class="t-divider">
    <section>...</section>

  </main>
</body>
```

### 16.2 Dashboard / Report page (financial topics)

Adds a KPI row immediately after the hero, before the first section:

```
<hero>
<section class="t-stat-grid"> stat blocks </section>
<hr class="t-divider">
<section> chart + table </section>
```

### 16.3 Editorial / press page

`press-research-communications/` uses a narrower max-width (760px), `--leading-airy` body, and a single accent (`--accent-digital`). No interactive widgets.

---

## 17. Subdirectory checklist

For every sub-directory that gets its own `index.html`:

1. Body class `page--[slug]` declared.
2. `--accent` set on that body class via the project accent (§3.3).
3. Topic page template (§16.1) used as the structural skeleton.
4. At least one section uses an editable component (input / slider / calculator) where domain data exists.
5. At least one section uses a visual diagram (timeline / flow / funnel / flywheel) where the topic has compound dynamics.
6. Tables use `.t-table`; cards use the canonical classes in §9.
7. Stats animate on viewport entry (`animNum`).
8. The sticky nav from General is reused; the active pill points to the current topic.

---

## 18. Anti-Patterns (Strictly Prohibited)

| Anti-pattern | Why |
|---|---|
| Light page background (white / cream) | The system is dark-first. Light only inside the actor flow diagram (§9.18). |
| Cormorant Garamond / Playfair / serif body fonts | The display face is Bebas Neue. Serifs were retired from the system. |
| Border radius > 4px on a generic card or button | Reserved for diagrams. A pill with `border-radius: 999px` for hover-able quick actions is forbidden in content. |
| Mixing 3 accents in a single card | One accent per card; semantic-role accents allowed only as marker dots, not as background. |
| Using emojis without a paired DM Mono label | Emojis need lexical context; never decorative. |
| Plain `<h1>` / `<h2>` without an eyebrow above | Every heading must be preceded by a `.t-eyebrow`. |
| Headings without Bebas Neue | All `h1`/`h2` are display face. `h3`/`h4` may use body face at weight 700. |
| Body text in Bebas Neue | Bebas is display-only. |
| Body sizes > 16px | Long-form copy is 11–15px. |
| Loading spinners | Use the linear `pulse` keyframe or the `animNum` counter pattern. |
| Drop shadows on plain content cards | Shadows are reserved for flow diagrams and the flywheel. |
| Gradient text fills | Gradient is only used on actor backgrounds and pattern card surfaces, never on text. |
| Bright stock photography | All imagery is desaturated, dimmed, or schematic. |
| Hardcoded hex values in component styles | Always reference a token. |
| Three accent colors in one chart | Charts carry the page accent + at most one semantic-role accent (success/risk). |
| Auto-playing audio / video | All media is muted + controls visible. |

---

## 19. CSS Custom Property Bootstrap

This block lives in `assets/css/tipolis-design-system.css` and is the **source of truth for tokens**. Every page inherits it automatically.

```css
/* See assets/css/tipolis-design-system.css for the full implementation. */
:root {
  /* Surface */
  --color-navy-1000: #0A1828;
  --color-navy-950:  #07111F;
  --color-navy-900:  #0D1E35;
  --color-navy-850:  #001E35;
  --color-navy-800:  #001829;
  --color-navy-750:  #002040;
  --color-navy-700:  #002A45;
  --color-navy-650:  #003050;
  --color-navy-600:  #00264A;
  --color-navy-500:  #1E3A5F;
  --color-navy-400:  #274C7A;

  /* Text on dark */
  --color-text-on-dark:        #FFFFFF;
  --color-text-on-dark-2:      #C8D4E0;
  --color-text-on-dark-muted:  #7A8A9E;
  --color-text-on-dark-faint:  #5A6A7A;
  --color-text-on-dark-fainter:#8A9AAE;

  /* Theme accents */
  --accent-treasury:  #FF6B35;
  --accent-digital:   #F5A623;
  --accent-bonds:     #3B82F6;
  --accent-strategic: #2ECC71;
  --accent-tools:     #0D9E8E;
  --accent-peer:      #A78BFA;
  --accent-warning:   #E74C3C;
  --accent-warning-2: #EF4444;
  --accent-cyan:      #06B6D4;

  /* Default page accent */
  --accent: var(--accent-treasury);

  /* Fonts */
  --font-display: 'Bebas Neue','Oswald','Impact',system-ui,sans-serif;
  --font-body:    'Jost','DM Sans',system-ui,sans-serif;
  --font-mono:    'DM Mono','JetBrains Mono','IBM Plex Mono',monospace;
}
```

---

## 20. Quick-Reference Cheat Sheet for AI Agents

When building any page in this repository:

1. **Read this file first.** It is loaded before tokens; tokens are loaded before components; components are loaded before content.
2. Add the required `<head>` block (§2). Confirm the three Google Fonts families are pre-connected and loaded.
3. Set `<body class="page--[topic-slug]">` and assign the topic's `--accent` (§3.3).
4. Open with a `.t-nav` (§11) — re-use General's nav with the active pill pointing to the current page.
5. Add the `.t-hero` block (§10). Hero h1 in Bebas Neue with one accent-colored sub-clause.
6. Build each section as eyebrow → h2 → lead → content blocks → `<hr class="t-divider">`. Never skip the eyebrow.
7. Pick components from §9. If a needed component does not exist there, build a new one in the same vocabulary and submit a PR to add it to this spec.
8. **Use tokens, never raw hex.** If a token doesn't cover your case, add one to §3.1 / §3.2 and to the CSS file.
9. **Default radius is 2px.** Larger radii are forbidden outside the flow diagram and the flywheel.
10. **Default body line-height is 1.65–1.8.** Default font-size is 12–14px for body, 10px for eyebrow.
11. Charts: SVG, 1.5px stroke, gradient fills 35→15%, mono labels in uppercase.
12. Numbers animate on entry via `animNum`.
13. Test on mobile: grids collapse to single column at 768px; flow diagrams stack; flywheel turns into a vertical list.
14. Run the **Anti-Pattern** checklist (§18) before opening the PR.

---

*This specification is derived from analysis of `general/index.html` (May 2026) as the canonical Tipolis visual artifact. It supersedes every ad-hoc styling decision made elsewhere in the repository.*
