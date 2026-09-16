# Design System Master File

> **LOGIC:** When building a specific page, first check `design-system/pages/[page-name].md`.
> If that file exists, its rules **override** this Master file.
> If not, strictly follow the rules below.

---

**Project:** WG Vidros
**Generated:** 2026-09-16 10:29:16
**Revised:** manually overridden after grounding in the client's reference (`Referencia.jpg`, brand case "KANTO" by Anna Malofeeva) — see rationale below
**Category:** Vidraçaria full-service (residencial + comercial), posicionamento premium/sofisticado
**Design Dials:** Variance 6/10 (Balanced / Modern) | Motion 8/10 (Complex) | Density 3/10 (Spacious)

---

## Why this overrides the auto-generated defaults

The `--design-system` search returned a generic "Luxury/Premium Brand" preset (light `#FAFAF9` background, Liquid Glass / Apple system-chrome style, Cormorant+Montserrat fashion pairing). That preset doesn't match the direction the client approved: a **dark-dominant, warm, photography-driven** mood extracted directly from the reference image, where color comes from ambient light and photography rather than from a flat palette. Spacing, shadow, motion mechanics and the checklist below are kept from the generated system — only palette, type and style are replaced.

### Color Palette

| Role | Hex | CSS Variable | Usage |
|------|-----|--------------|-------|
| Background | `#15110D` | `--color-bg` | Base page color — warm near-black espresso, not pure black |
| Surface | `#211A14` | `--color-surface` | Raised panels, cards, nav-on-scroll |
| Surface Deep | `#0D0A08` | `--color-surface-deep` | Sidebar bars, footer, the vertical-wordmark rail |
| Foreground | `#F3EDE4` | `--color-fg` | Primary text on dark — warm off-white, never pure `#FFF` |
| Muted Foreground | `#A99C8C` | `--color-fg-muted` | Secondary text, eyebrow labels, captions |
| Border | `#3A2F26` | `--color-border` | Hairlines on dark (or `rgba(243,237,228,.12)`) |
| Accent | `#C89456` | `--color-accent` | Brass/warm-gold — CTA fills, active states, the "light" of the site |
| On Accent | `#15110D` | `--color-on-accent` | Dark text on the gold accent — reads as brushed metal, not a flat sticker |
| Breather BG | `#EFE9DF` | `--color-bg-light` | The occasional light section (quote, form) — warm stone, not white |
| Breather FG | `#1B140F` | `--color-fg-on-light` | Text on the breather sections |
| Destructive | `#E4573D` | `--color-destructive` | Form errors — warm red-orange, stays inside the palette family |
| On Destructive | `#FFFFFF` | `--color-on-destructive` | |

**Color Notes:** No flat "brand color." Per the reference's own logic, the photography and ambient light supply the color; the UI stays a warm, near-monochrome dark shell. Accent gold is used sparingly — CTAs, active nav state, hairline highlights — never as a large fill.

**Dark-surface shadows:** drop-shadows are invisible on `#15110D`. Elevation on dark panels is expressed with a 1px lighter border (`--color-border`) plus a subtle inner highlight, not `box-shadow` alone. Reserve real shadows for elements sitting on the light breather sections.

### Typography

- **Heading / display / the rotated wordmark:** Space Grotesk (500–600) — geometric, slightly technical, holds up wide-tracked in caps; reads architectural rather than "fashion-luxury"
- **Body / UI / labels:** Inter (400–500) — neutral, highly legible at small sizes, the workhorse under the display font
- **Mood:** editorial, architectural, quietly premium — deliberately not a serif luxury pairing (Playfair/Cormorant), which would undercut the technical credibility a glass fabricator needs
- **Google Fonts:** [Space Grotesk + Inter](https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&family=Inter:wght@400;500;600&display=swap)

**CSS Import:**
```css
@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&family=Inter:wght@400;500;600&display=swap');
```

**Scale (fluid, clamp-based):**

| Role | Mobile → Desktop | Weight | Line-height | Tracking |
|------|---|---|---|---|
| Hero display | `clamp(2.5rem, 6vw, 5.5rem)` | 500 | 1.05 | -0.02em |
| H2 section | `clamp(1.75rem, 3.5vw, 3rem)` | 500 | 1.1 | -0.01em |
| H3 | `clamp(1.25rem, 2vw, 1.5rem)` | 500 | 1.3 | 0 |
| Eyebrow / label | `0.75rem` fixed | 500 (Inter) | 1.4 | 0.15em, uppercase |
| Body | `1rem–1.0625rem` | 400 (Inter) | 1.6 | 0 |
| Pill badge | `0.8125rem` fixed | 500 (Inter) | 1.3 | 0.02em |

### Spacing Variables

*Density: 3/10 — Spacious*

| Token | Value | Usage |
|-------|-------|-------|
| `--space-xs` | `4px` / `0.25rem` | Tight gaps |
| `--space-sm` | `8px` / `0.5rem` | Icon gaps, inline spacing |
| `--space-md` | `24px` / `1.5rem` | Standard padding |
| `--space-lg` | `32px` / `2rem` | Section padding |
| `--space-xl` | `48px` / `3rem` | Large gaps |
| `--space-2xl` | `64px` / `4rem` | Section margins |
| `--space-3xl` | `96px` / `6rem` | Hero padding |

### Shadow Depths

| Level | Value | Usage |
|-------|-------|-------|
| `--shadow-sm` | `0 1px 2px rgba(0,0,0,0.05)` | Subtle lift |
| `--shadow-md` | `0 4px 6px rgba(0,0,0,0.1)` | Cards, buttons |
| `--shadow-lg` | `0 10px 15px rgba(0,0,0,0.1)` | Modals, dropdowns |
| `--shadow-xl` | `0 20px 25px rgba(0,0,0,0.15)` | Hero images, featured cards |

---

## Component Specs

### Buttons

**Revisado após o primeiro rascunho:** o cliente pediu pra suavizar as quinas do
site inteiro. `--raio-sm` foi de 2px pra 8px e `--raio` de 4px pra 16px — os
valores abaixo já refletem a versão atual, não mais "quase reto".

```css
/* Primary Button — the brass/gold accent, used once per view max */
.btn-primary {
  background: #C89456;
  color: #15110D;
  padding: 14px 28px;
  border-radius: 8px; /* --raio-sm — suave, não mais quase reto */
  font-family: 'Space Grotesk', sans-serif;
  font-weight: 500;
  letter-spacing: 0.02em;
  transition: background 250ms ease, transform 250ms ease;
  cursor: pointer;
}

.btn-primary:hover {
  background: #D9A868;
  transform: translateY(-1px);
}

/* Secondary Button — outline, sits on dark */
.btn-secondary {
  background: transparent;
  color: #F3EDE4;
  border: 1px solid #3A2F26;
  padding: 14px 28px;
  border-radius: 8px;
  font-weight: 500;
  transition: border-color 250ms ease, background 250ms ease;
  cursor: pointer;
}

.btn-secondary:hover {
  border-color: #C89456;
  background: rgba(200, 148, 86, 0.08);
}

/* Cápsula — exceção só no CTA principal do herói, raio 999px (pill cheia).
   Não espalhar pros outros botões, ver assets/estilo.css seção 5. */
```

### Pill Badges (signature device — floating diferenciais over photography)

```css
.pill-badge {
  display: inline-flex;
  align-items: center;
  background: rgba(21, 17, 13, 0.55);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid rgba(243, 237, 228, 0.18);
  border-radius: 999px;
  padding: 8px 16px;
  font-size: 0.8125rem;
  color: #F3EDE4;
  letter-spacing: 0.02em;
}
```
The only deliberate glassmorphism moment in the system — frosted glass standing over a photo of actual glass. Use on floating differentiator badges and the on-scroll nav bar only; do not spread the frosted-blur treatment to cards or modals, or it stops reading as intentional.

### Cards

```css
.card {
  background: #211A14;
  border: 1px solid #3A2F26;
  border-radius: 4px;
  padding: 32px;
  transition: border-color 250ms ease, transform 250ms ease;
}

.card:hover {
  border-color: #C89456;
  transform: translateY(-2px);
}
```

### Inputs

```css
.input {
  background: transparent;
  padding: 14px 16px;
  border: 1px solid #3A2F26;
  border-radius: 2px;
  color: #F3EDE4;
  font-size: 16px; /* keeps iOS from auto-zooming */
  transition: border-color 200ms ease;
}

.input::placeholder { color: #A99C8C; }

.input:focus {
  border-color: #C89456;
  outline: none;
  box-shadow: 0 0 0 3px rgba(200, 148, 86, 0.15);
}
```

### Modals

```css
.modal-overlay {
  background: rgba(13, 10, 8, 0.72);
  backdrop-filter: blur(6px);
}

.modal {
  background: #211A14;
  border: 1px solid #3A2F26;
  border-radius: 4px;
  padding: 32px;
  box-shadow: var(--shadow-xl);
  max-width: 500px;
  width: 90%;
}
```

---

## Style Guidelines

**Style:** Dark Editorial / Parallax Storytelling (base) + Glassmorphism (accent only, see Pill Badges above)

**Keywords:** cinematic photography, warm ambient light as the only color source, scroll-driven chapters, generous negative space, thin wide-tracked type punctuated by one high-contrast moment per view

**Best For:** Brand storytelling for a physical, tactile product (glass) where the craft is best sold through mood and light, not a spec sheet

**Three signature devices, carried from the reference (`Referencia.jpg` / KANTO) and re-applied to glass:**
1. **Vertical wordmark rail** — "WG VIDROS" rotated 90°, running the height of the viewport on a `--color-surface-deep` bar. Repeats across every chapter as a visual anchor / reading-progress cue.
2. **Floating pill badges over photography** — short diferenciais ("Vidro Temperado", "Garantia", "Instalação em 48h") in frosted-glass pills scattered over hero/chapter images, never in a plain list.
3. **Photography carries the color.** UI chrome stays near-monochrome dark; the only saturation on screen comes from glass reflections, warm LED/golden-hour light in the photos, and the brass accent on CTAs.

**Key Effects:** scroll-triggered chapter reveals, subtle parallax on background photography (foreground content stays static — see Motion), frosted-glass badges as the one literal "glass" material cue.

### Page Pattern

**Pattern Name:** Scroll-Triggered Storytelling

- **Conversion Strategy:** Keep the narrative understandable without scroll-driven effects. Use progress indicator. Mobile: simplify animations. Keep DOM reading order complete; disable parallax and scroll-scrub under reduced motion. Pause scroll animation when offscreen or hidden and render each chapter in its final readable state under reduced motion.
- **CTA Placement:** End of each chapter (mini) + Final climax CTA
- **Section Order:** Intro hook > Chapter 1 (problem) > Chapter 2 (journey) > Chapter 3 (solution) > Climax CTA

---

## Motion

This is a single-page scroll site, not a multi-route app — so the primary motion is **chapter reveal on scroll**, not page transition. Use GSAP + ScrollTrigger: each chapter's photo does a slow `scale(1.0 → 1.06)` drift (foreground text stays fixed, no translateY on text) as it enters, pills fade/rise in staggered by ~60ms, eyebrow labels animate a simple opacity+translateY(8px). Duration 600-900ms, `power2.out` on the way in.

`Flip` (below) is reserved for one optional moment: an obras/projects gallery where clicking a thumbnail expands it in place.

```js
const state = Flip.getState('.gallery-item'); openLightbox(item); Flip.from(state, { duration: 0.6, ease: 'expo.inOut', absolute: true, zIndex: 100 });
```

**Framework notes:** Requires the GSAP Flip plugin; the 'from' and 'to' state must render the same element with a shared data-flip-id; use `matchMedia('(prefers-reduced-motion: reduce)')` to skip non-essential motion and render the final state immediately — under reduced motion every chapter renders in its resting state with no scale drift or stagger.

---

## Anti-Patterns (Do NOT Use)

- ❌ Cheap visuals
- ❌ Fast animations

### Additional Forbidden Patterns

- ❌ **Emojis as icons** — Use SVG icons (Heroicons, Lucide, Simple Icons)
- ❌ **Missing cursor:pointer** — All clickable elements must have cursor:pointer
- ❌ **Layout-shifting hovers** — Avoid scale transforms that shift layout
- ❌ **Low contrast text** — Maintain 4.5:1 minimum contrast ratio
- ❌ **Instant state changes** — Always use transitions (150-300ms)
- ❌ **Invisible focus states** — Focus states must be visible for a11y

---

## Pre-Delivery Checklist

Before delivering any UI code, verify:

- [ ] No emojis used as icons (use SVG instead)
- [ ] All icons from consistent icon set (Heroicons/Lucide)
- [ ] `cursor-pointer` on all clickable elements
- [ ] Hover states with smooth transitions (150-300ms)
- [ ] Text contrast 4.5:1 minimum, checked separately on dark base AND on the light breather sections
- [ ] Focus states visible for keyboard navigation (use the accent gold as the focus ring on dark)
- [ ] `prefers-reduced-motion` respected
- [ ] Responsive: 375px, 768px, 1024px, 1440px
- [ ] No content hidden behind fixed navbars
- [ ] No horizontal scroll on mobile
