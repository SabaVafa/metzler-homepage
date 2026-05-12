# Developer Handoff — Metzler Homepage Redesign

**Date:** 12 May 2026
**Deliverable:** Static homepage (`index.html` + `styles.css` + inline JS) for `edelstahl-tuerklingel.de`
**Scope:** Frontend visual + IA redesign. No backend, no CMS integration.

---

## 1. Files & Tech Stack

| File | Purpose |
|---|---|
| `index.html` | Markup, inline `<script>` block at end of `<body>` |
| `styles.css` | All styles (no preprocessor) |
| `Logo/` | Metzler logos (red+schwarz / red+weiß SVGs) |
| `ICONS/` | profile, cart, Star, StarRating, Trustedshop |
| `XDM10 Banner/` | Photo.png + Red dot.svg |
| `Trust badges/` | NTV.svg, Vergleich3.svg, Reddot2.svg, Trustedshop 2.svg, Garantie2.svg, customers.svg + `Top SHOP/` subfolder (Label1–4.png) |
| `Reviews/` | Google.svg, TrustedShop.svg, Trust Pilot.svg |
| `images/` | 11 category photos (Briefkasten, Sprechanlage, Hochbeet, etc.) |
| `Awards/` | NTV award assets |
| `Carousel Images/` | Customer photos |
| `Paymentshipping logos/payment/` | SEPA, AMEX, Visa, Klarna, PayPal, MasterCard, Apple Pay, Google Pay, Amazon Pay, Vorkasse |

- **No build step.** Pure HTML + CSS + vanilla JS (~15 lines inline for mobile nav toggle).
- **No framework.** No React, Vue, Tailwind, etc. If migrating, base CSS variables in `:root` can be ported directly to design tokens.
- **No package.json / dependencies.**

---

## 2. Design System Tokens

Defined in `:root` at the top of `styles.css`:

```css
/* Colors */
--white: #FFFFFF
--paper-white: #F5F6FA
--graphite-300: #DADADA
--schwarz: #1A171B
--teal: #015253          /* brand primary */
--teal-bright: #006D75   /* hero CTA */
--teal-mint: #5CDBD3     /* accent */
--deep-teal: #01292A
--rot: #D42924

/* Type scale (Helvetica Neue throughout) */
--text-display: 2.875rem   /* 46 */
--text-h1: 1.875rem        /* 30 */
--text-h2: 1.5rem          /* 24 */
--text-h3: 1.25rem         /* 20 */
--text-h4: 1.125rem        /* 18 */
--text-body: 1rem          /* 16 */
--text-body-sm: 0.875rem   /* 14 */
--text-caption: 0.75rem    /* 12 */

/* Spacing — 4px base */
--sp-2 = 8px
--sp-3 = 12px
--sp-4 = 16px
--sp-5 = 20px
--sp-6 = 24px
--sp-8 = 32px
--sp-10 = 40px
--sp-12 = 48px
--sp-16 = 64px

/* Radii */
--r-s = 2px
--r-m = 4px
--r-l = 8px
--r-pill = 999px

/* Container */
--container: 1380px (≤1440), 1560px (≥1440), 1760px (≥1920), 2040px (≥2560)
```

---

## 3. Responsive Breakpoint System

| Breakpoint | Behavior |
|---|---|
| `min-width: 1024px` | **Desktop Lock** — forces all desktop chrome, overrides any leaked mobile styles |
| `min-width: 1440px` | Container widens to 1560px, gutter padding scales up |
| `min-width: 1920px` | Container widens to 1760px, hero stage anchors `left: 50%`, gutter scales further |
| `min-width: 2560px` | Container caps at 2040px |
| `max-width: 1280px` | Hero stage shifts to `left: 60%`, copy max-width 620px |
| `max-width: 1100px` | Hero stacks vertically (copy → product), trust bar 6→3 cols, footer 5→3 cols |
| `max-width: 900px` | Header collapses (search becomes full-width below logo row), category grid → 4-col bento, FAQ stacks, reviews stack |
| `max-width: 720px` | **Mobile** — hamburger menu activates, sticky bottom action bar, trust bar 6→2 cols |
| `max-width: 640px` | Small mobile — section spacing tightens, bento grid 90px row unit |

**Critical:** the `@media (min-width: 1024px)` "Desktop Lock" block uses `!important` to guarantee no mobile rule leaks above 1024px. Do not remove this block unless you audit every mobile media query carefully.

---

## 4. Component Inventory

| Component | Selector | Notes |
|---|---|---|
| Topbar | `.topbar` | 5 trust claims (left) + Kontakt/Versand links (right). Teal `#015253` bg |
| Header | `.header` → `.header__main` | Logo + search bar + profile/cart icons. Grid: `auto 1fr auto` × 80px |
| Hamburger | `.header__burger` | ≤720px only — toggles `.mobile-nav` slide-in panel |
| Main nav | `.header__nav` | 9 categories distributed `space-between` |
| Mobile nav panel | `.mobile-nav` + `.mobile-nav__panel` | Fixed slide-in from left, 86vw / 380px max |
| Mobile bar | `.mobile-bar` | Sticky bottom: Suchen / Beratung (tel:) / Warenkorb |
| Hero banner | `.hero-banner` | Monolithic canvas (`#0E0F12`), copy on left + product on right (absolute, mix-blend-mode: screen) |
| Trust bar | `.trustbar` → `.trustbar__inner` | 6 badges in tile-styled cards. Mobile (<768px) becomes swipe carousel |
| Category grid | `.cats__grid` → `.cat-card--c1`…`c11` | Desktop: 4×6 sub-row asymmetric grid. Mobile (≤900px): 4-col Bento Rhythm with explicit row/col placement |
| Why Metzler | `.why` → `.why__grid` | 3-up card row |
| Awards | `.awards` | NTV "Deutschlands beliebtester Anbieter", dark-teal section |
| Reviews | `.reviews` → `.reviews__head` + `.reviews__tabs` + `.reviews__grid` | Filter tabs (Alle / Google / TrustedShops / Trustpilot) + 3 review cards. Tabs become horizontal swipe carousel on mobile with mask-image fade |
| FAQ | `.faq` → `.faq__inner` | Sidebar (`.faq__support`) + details/summary accordion list. Mobile reorders sidebar AFTER list via `flex order` |
| Footer | `.footer` → `.footer__top` | 5-column layout: Brand+Labels / Hotlines / Informationen / Service / Follow us+Qualität |

---

## 5. JavaScript

A single inline `<script>` block before `</body>` handles the mobile nav toggle. ~15 lines, no dependencies.

```js
(function(){
  var burger = document.getElementById('navBurger');
  var panel  = document.getElementById('mobileNav');
  if (!burger || !panel) return;
  function open() { /* aria + body scroll lock */ }
  function close() { /* aria reset + body scroll unlock */ }
  burger.addEventListener('click', open);
  panel.querySelectorAll('[data-close], a').forEach(function(el){ el.addEventListener('click', close); });
  document.addEventListener('keydown', function(e){ if(e.key==='Escape') close(); });
})();
```

**ARIA:** burger has `aria-expanded` toggled, panel has `aria-hidden`. Escape key closes.

---

## 6. Key Design Decisions

| Decision | Rationale |
|---|---|
| Hero uses `mix-blend-mode: screen` on Photo.png | Dissolves the photo's dark background into the canvas so the device appears to emerge from the same atmosphere as the typography. No visible image rectangle. |
| `mask-image: linear-gradient` on photo left edge | Feathers the device's spotlight halo into the canvas, eliminating the vertical light streak seen in earlier iterations. |
| `-webkit-box-reflect` for floor reflection | Mirror reflection beneath product with smooth 20%→0% linear opacity decay. WebKit-only; on Firefox this gracefully degrades (no reflection, no breakage). |
| `clamp(min, preferred, max)` everywhere | Container padding, hero copy margin, font sizes all use clamp. Single source-of-truth scaling. |
| Hero copy `margin-left: max(...)` formula | Mirrors `.container`'s effective content edge so hero text x-position matches header/nav/trust bar exactly at every viewport width. |
| Category grid Bento Rhythm on mobile | Explicit row/col placement for each tile (no auto-flow). Pattern: HERO → Pair → Tall+Stack → HERO → Tall+Stack → Closure. |
| Trust bar mobile carousel | `scroll-snap-type: x mandatory` + `mask-image` fade on both edges. Peek effect signals swipe. |
| Reviews tabs mobile carousel | Same scroll-snap + mask-image pattern. Tabs keep desktop segmented-control look. |
| All copy in German | Title, body, CTAs, badges. UWG-compliant claims throughout. |
| Honest "Bis zu 10 Jahre Garantie" | Not "auf alle Produkte 10 Jahre" — explicit scope to Edelstahl, statutory warranty referenced for electronics. |
| Hikvision removed from headline copy | Brand authorship preserved without promoting partner. |

---

## 7. Browser Support

- **Tested:** Chrome 130+, Edge 130+, Firefox 130+, Safari 17+
- **Known limitations:**
  - `-webkit-box-reflect` (product reflection) — Chromium + Safari only. Firefox shows device without reflection (graceful degradation).
  - `mask-image` in mobile carousels — supported in all modern browsers; for IE11 there is no support but IE11 is out of scope.
  - `backdrop-filter` on the mobile nav backdrop — supported everywhere modern.

---

## 8. Asset Inventory & Replacement Notes

Images are referenced with URL-encoded paths because folder names contain spaces (e.g., `Trust%20badges/`). When adding new assets:

- Keep folder structure intact (don't rename `Trust badges`, `XDM10 Banner`, etc.)
- Or rename folders to kebab-case AND update CSS/HTML references accordingly
- SVGs preferred for icons; PNGs for photographs
- All product photography under 500KB recommended; consider WebP conversion before production deploy

---

## 9. Accessibility

- Semantic HTML5 landmarks: `<header>`, `<nav>`, `<main>`, `<section>`, `<aside>`, `<footer>`
- ARIA labels on icon buttons (`aria-label="Mein Konto"`, `aria-label="Warenkorb"`)
- Hamburger button: `aria-expanded`, `aria-controls`, panel has `aria-hidden`, dialog `role="dialog"`
- FAQ accordion uses native `<details>`/`<summary>` (keyboard accessible, screen-reader accessible)
- All decorative icons marked `aria-hidden="true"`
- `lang="de"` on `<html>` enables proper German hyphenation
- Color contrast: white on dark teal (≥7:1), Schwarz on Paper-White (≥12:1), all text passes WCAG AA

**Known accessibility gaps for future work:**
- Focus rings on icon buttons / nav links could be more visually distinct
- Mobile-bar action buttons don't have keyboard-trap handling
- No skip-to-content link

---

## 10. Open Items & Next Phase

These are deliberately **not** implemented — they require product decisions, not design:

| Item | Owner | Status |
|---|---|---|
| Journey segmentation strip (homeowner / architect / B2B) | Product | Decision needed |
| Lead capture: PDF Katalog download with email gate | Marketing + Product | Email infra needed |
| "Beratung buchen" CTA wiring to actual booking system | Product | Booking system needed |
| Cross-sell "Eingangs-Set" bundle module | Product + Marketing | Bundle pricing TBD |
| Quantified "Drei Gründe" copy (specific numbers) | Operations + Legal | Source data needed |
| Reviews v2: surface 6 instead of 3, add product context | Customer success | Content production |
| Mobile hero rebuild (currently strips product image at ≤960px) | Design + Dev | Awaiting product photography for mobile |

---

## 11. Quick-Start Checklist for the Receiving Dev

1. Open `index.html` directly in browser — no server needed
2. Verify viewport behaviors at: 375 / 768 / 1024 / 1440 / 1920
3. Confirm hamburger appears ≤720px and the Desktop Lock at ≥1024px hides it
4. Click through all CTAs (placeholders — wire to real endpoints)
5. Replace placeholder `tel:+4971213177310` with actual hotline if different
6. Replace placeholder review content with verified production reviews
7. Audit lazy-loading: add `loading="lazy"` to all images below the first viewport (currently absent)
8. Add analytics events to CTAs before launch
9. Run Lighthouse: expect performance >80, accessibility >90, SEO >95
10. Production deploy: minify CSS, optimize images, enable Brotli compression

---

## 12. Contact

- **Original brief:** Metzler Homepage Redesign — premium UX/UI/CRO redesign matching `metzlerdesignsystem.netlify.app`
- **User contact:** s.vafakhah@metzlergmbh.de

---

**Handoff status:** Ready for development integration. All visual + IA work complete; structural CRO work (segmentation, lead capture, bundles) flagged for next phase.
