# Metzler Homepage — Components & Design System

Reference for the design tokens and reusable UI components defined in `styles-v2.css`. Open `components.html` in a browser for the visual preview.

---

## Breakpoint contract

| Tier    | Range            | Notes                                        |
| ------- | ---------------- | -------------------------------------------- |
| Mobile  | `≤ 767px`        | Burger header, 1-col stacks, mobile bento    |
| Tablet  | `768 – 1023px`   | Sticky header, 2-col reviews, simplified UI  |
| Desktop | `≥ 1024px`       | Full nav strip, multi-col grids, large hero  |

**All new responsive rules must target one of these three tiers.** Legacy queries (`≤720`, `≤900`, `≤1100`, `≤1280`) remain in place and should be migrated section-by-section. Do not introduce non-standard bounds.

---

## Design tokens

All tokens live on `:root` at the top of `styles-v2.css`.

### Colors — primary

| Token            | Value                       | Use                                  |
| ---------------- | --------------------------- | ------------------------------------ |
| `--teal`         | `#015253`                   | Primary brand colour                 |
| `--teal-dark`    | `#01292A`                   | Dark backgrounds (hero, awards)      |
| `--teal-hover`   | `#006D75`                   | Hover state on teal CTAs             |
| `--teal-mint`    | `#5CDBD3`                   | Accent (eyebrows, hover indicators)  |
| `--teal-5`       | `rgba(1,82,83,0.05)`        | Tinted hover backgrounds             |
| `--teal-10`      | `rgba(1,82,83,0.10)`        | Tinted focus rings                   |

### Colors — brand

| Token        | Value      | Use                       |
| ------------ | ---------- | ------------------------- |
| `--rot`      | `#D42924`  | Logo red, accent          |
| `--schwarz`  | `#1A171B`  | Primary text colour       |

### Colors — neutrals

| Token             | Value      | Use                                |
| ----------------- | ---------- | ---------------------------------- |
| `--black`         | `#000000`  | Stroke / overlays                  |
| `--white`         | `#FFFFFF`  | Backgrounds                        |
| `--graphite-500`  | `#A1A1A1`  | Secondary text, icon strokes       |
| `--graphite-300`  | `#DADADA`  | Borders, separators                |
| `--graphite-100`  | `#F0F0F0`  | Subtle backgrounds                 |
| `--paper-white`   | `#F5F6FA`  | Trustbar / Reviews section bg      |

### Typography scale

| Token             | Size              | Used for                  |
| ----------------- | ----------------- | ------------------------- |
| `--text-display`  | `2.875rem` (46px) | Section titles (desktop)  |
| `--text-h2`       | `1.5rem` (24px)   | Cat-card titles, headings |
| `--text-h3`       | `1.25rem` (20px)  | Sub-headings              |
| `--text-h4`       | `1.125rem` (18px) | Subtitles, large body     |
| `--text-body`     | `1rem` (16px)     | Default body, buttons     |
| `--text-body-sm`  | `0.875rem` (14px) | Captions, meta            |
| `--text-caption`  | `0.75rem` (12px)  | Eyebrows, badges          |

Line-heights: `--lh-display: 1.15`, `--lh-heading: 1.3`, `--lh-body: 1.5`.

**Section-title responsive scale:**
| Tier    | Size |
| ------- | ---- |
| Mobile  | 32px (`2rem`) |
| Tablet  | 36px (`2.25rem`) |
| Desktop | 46px (`--text-display`) |

### Spacing scale

| Token     | Px  |
| --------- | --- |
| `--sp-1`  | 4   |
| `--sp-2`  | 8   |
| `--sp-3`  | 12  |
| `--sp-4`  | 16  |
| `--sp-5`  | 20  |
| `--sp-6`  | 24  |
| `--sp-8`  | 32  |
| `--sp-10` | 40  |
| `--sp-12` | 48  |
| `--sp-16` | 64  |
| `--sp-20` | 80  |
| `--sp-24` | 96  |

### Radii

| Token       | Value       | Use                                   |
| ----------- | ----------- | ------------------------------------- |
| `--r-s`     | 2px         | Hero chips                            |
| `--r-m`     | 4px         | Buttons, scroll arrows                |
| `--r-l`     | 8px         | Reviews bar, FAQ support card         |
| `--r-xl`    | 12px        | Cat cards                             |
| `--r-pill`  | 9999px      | Icon buttons, badges, "Alle" button   |

### Shadows

| Token         | Use      |
| ------------- | -------- |
| `--shadow-lg` | Drawers  |

Other shadow values used on the page are written inline (not via tokens) — see `.awards__photo`, `.hero-banner__poster`, etc.

### Layout

| Token           | Default               |
| --------------- | --------------------- |
| `--container`   | `1380px` (max-width)  |
| `--gutter`      | `24px`                |
| `--section-y`   | `80px` (vertical section padding — drops to `64px` at tablet, `48px` at mobile `≤640`) |

Container side-padding: `clamp(24px, 4vw, 56px)` (tightens at narrow viewports).

---

## Components

> **Devices column legend:** ▣ Mobile · ◆ Tablet · ● Desktop. A symbol means the component is rendered/visible at that breakpoint.

### Buttons

Base selector: `.btn` — 48px tall, 24px horizontal padding, `--r-m` radius, 700 weight body text. **Devices: ▣ ◆ ●** (universal — appears in hero CTAs, awards CTAs, FAQ, footer everywhere).

| Selector                | Style                                            | Devices  |
| ----------------------- | ------------------------------------------------ | -------- |
| `.btn--primary`         | Teal fill, white text. Hover → `--teal`.         | ▣ ◆ ●    |
| `.btn--secondary`       | Transparent + 1.5px teal border, teal text.      | ▣ ◆ ●    |
| `.btn--secondary-light` | Transparent + 1.5px white border, white text.    | ▣ ◆ ●    |
| `.btn--arrow`           | Adds chevron, reduces right padding.             | ▣ ◆ ●    |
| `.btn--lg`              | 56px tall, larger padding + `--text-h4` font.    | ▣ ◆ ●    |

### Text links

`.tlink` — inline-flex teal link with a 1.5px underline accent. Modifier `.tlink--mint` for dark backgrounds (awards section). **Devices: ▣ ◆ ●**

### Icon button

`.icon-btn` — 52×52 pill-shaped wrapper for SVG icons (cart, profile). 28×28 glyph inside.

**Devices: ▣ ◆ ●** — sizing differs:
- Mobile (`≤720`): collapses to 48×48 with 22×22 glyphs.
- Tablet + Desktop: 52×52 with 28×28 glyphs (default).

### Trust badge

`.trust` — vertical stack with framed icon + title + subtitle. Used in the trustbar. Hairline separator between siblings via `.trust + .trust::before`.

**Devices: ▣ ◆ ●** — layout differs:
- Mobile / Tablet (`≤1023`): horizontal scrollable carousel; cards `flex 0 0 22–24%`, prev/next arrows + edge fades on tablet.
- Desktop: static 6-column grid.

### Category card

`.cat-card` — image-fill card with overlay scrim and title text. Per-card modifiers `.cat-card--c1` through `.cat-card--c11`.

**Devices: ▣ ◆ ●** — layout differs:
- Mobile / Tablet (`≤1023`): mobile bento via `grid-template-areas` — 3 stacked modules of full-width hero + 2-col row.
- Desktop: 4×6 grid with explicit per-card `grid-column` / `grid-row` placement.

Variants (universal):
- `.cat-card__discount` — top-left red pill ("5% Rabatt", "10% Rabatt").
- `.cat-card__title::before` — animated teal-mint underline on hover (desktop only; hidden on touch to avoid sticky-press visual).

### Review card

`.review` — white card with platform logo (top-right), avatar, stars, name, quote, timestamp. Border + 8px radius.

**Devices: ▣ ◆ ●** — grid layout differs:
- Mobile (`≤900`): 1 column.
- Tablet (`768–1023`): 2 columns.
- Desktop: 3 columns.

### Reviews tabs

`.reviews__tabs` — segmented control. Outer `.reviews__tabs`: 8px radius, white background, hairline border. Inner `.reviews__tab`: 6px radius (concentric), transparent base, white when `[aria-selected="true"]`.

**Devices: ▣ ◆ ●** — layout differs:
- Mobile + Tablet (`≤1023`): nowrap horizontal scroll with scroll-snap.
- Desktop: full segmented bar.

### Hero chip

`.hero-chip` — pill badge used in the hero ("POWERED BY HIKVISION", "DESIGNED IN GERMANY"). 28px tall, 12px padding, `--r-s` radius, uppercase caption. **Devices: ▣ ◆ ●**

### Awards credential

`.awards__credential` — paired badge (medallion image) + text block (label + meta) anchored above the awards headline.

**Devices: ▣ ◆ ●** — sizing scales:
- Mobile: 88px badge, 12/14px label/meta (defaults).
- Tablet: 148px badge, 16/18px label/meta.
- Desktop: 128px badge, 14/16px label/meta.

### Hairline separator

Used in trustbar between cards and the top divider: linear gradient with `--graphite-300`. **Devices: ◆ ●** (mobile carousel re-positions it; not used as a section divider on mobile).

---

## Header

| State                            | Layout                                                |
| -------------------------------- | ----------------------------------------------------- |
| Mobile (`≤720`)                  | Burger + search-icon (left) · logo (center) · cart + profile (right). Drawer slides in from left. |
| Tablet (`768–1023`)              | Logo · search bar · cart + profile. **Permanently sticky.** "Alle" pill in the category strip opens drawer (anchored below header, no backdrop, chevrons on rows). |
| Desktop (`≥1024`) — at rest      | Top row: logo + search + icons. Strip: 9 categories. |
| Desktop (`≥1024`) — scrolled     | Top row gains compact **"Kategorien"** pill. Strip hides. Drawer opens as anchored dropdown. |

### Drawer

`.mobile-nav` — fixed full-viewport container with backdrop + slide-from-left panel. On desktop & tablet, the panel is restyled to anchor below the header (top: 80px / 72px) with no backdrop, chevron-right on each row, and toggle-on-click behavior via `[data-nav-toggle]`.

---

## Section rhythm

`.section { padding-block: var(--section-y); }`

| Tier    | `--section-y` |
| ------- | ------------- |
| Mobile `≤640` | `--sp-12` (48px) |
| Mobile `641–767` | `--sp-16` (64px) |
| Tablet  | `--sp-16` (64px) |
| Desktop | `--sp-20` (80px) |

The awards section additionally has an explicit `padding-block: var(--sp-16)` at tablet to guarantee symmetric top/bottom whitespace.

---

## JavaScript

A single `<script>` block at the bottom of `index.html` powers the interactive bits:

| IIFE                           | Purpose                                                  |
| ------------------------------ | -------------------------------------------------------- |
| Mobile + tablet nav toggle     | Opens/closes `#mobileNav` via `[data-nav-toggle]`. Closes on link click, `Esc`, or backdrop click. |
| Tablet nav fit                 | At tablet widths, hides category links that don't fit on one line. Hidden items remain in the burger drawer. |
| Trustbar prev/next             | Scroll arrows + edge fades for the tablet trustbar carousel. Auto-toggles at start/end. |
| Scroll-shrink                  | Adds `body.is-scrolled` at any scroll position. Powers the desktop Kategorien-pill reveal. |
| Search placeholder             | Three-tier responsive placeholder (mobile: "Produkte suchen…"; tablet: 2 items; desktop: 3 items). |
| Search icon stub               | Logs a hint; ready for a future modal wire-up.           |
| Mobile-bar safe area           | (legacy) — no-op now that the bar is removed.            |

---

## File map

| File                   | Purpose                                       |
| ---------------------- | --------------------------------------------- |
| `index.html`           | Single-page markup                            |
| `styles-v2.css`        | Single stylesheet (no build step)             |
| `components.html`      | Visual reference of components (this folder)  |
| `COMPONENTS.md`        | Token + component reference (this file)       |
| `ICONS/`               | SVG icons (cart, profile, menu, search, etc.) |
| `Logo/`                | Brand marks (Metzler logo, two variants)      |
| `images/`              | Category card photos (desktop)                |
| `images/Mobile/`       | Category card photos (mobile, ≤768 swap)      |
| `Awards/`              | Award badges                                  |
| `Trust badges/`        | Trustbar icons                                |
| `Reviews/`             | Review platform logos                         |
| `Carousel Images/`     | Awards photo                                  |
| `XDM10 Banner/`        | Hero device + reddot                          |
| `Paymentshipping logos/` | Footer payment & delivery logos             |

---

## Conventions

1. **No build step.** Edit `index.html` and `styles-v2.css` directly. Hard-refresh (Ctrl+F5) after CSS edits — GitHub Pages and browsers cache aggressively.
2. **Stylesheet filename** is `styles-v2.css` (renamed mid-project to defeat a stale CDN cache). The `<link>` in `index.html` already points to it.
3. **All assets** referenced by the HTML are tracked in git. Untracked files in the working folder are experiments — safe to delete.
4. **Mobile and desktop layouts are stable.** Tablet (768–1023) was the most recent pass; expect minor visual tuning there before any major changes.
