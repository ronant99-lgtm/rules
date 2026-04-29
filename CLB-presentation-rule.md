---
title: CLB Presentation Rule
version: 1.1
source: masque_theme_CLB.pptx (Institut Léon Bérard)
date: 2026-04-22
applies_to: PowerPoint (.pptx), HTML presentations
---

# CLB Presentation Rule
**Institut Léon Bérard — Design System for Professional Presentations**

---

## 1. Canvas & Format

| Property | Value |
|----------|-------|
| Aspect ratio | **16:9** |
| Slide size | 33.87 cm × 19.05 cm (13.33″ × 7.50″) |
| Background (content slides) | **White `#FFFFFF`** |
| Background (dark slides) | **Navy `#2C426C`** — section breaks, end screens |

---

## 2. Color Palette

### Brand colors

| Role | Name | Hex |
|------|------|-----|
| **Primary** | CLB Navy | `#2C426C` |
| **Accent 1** | CLB Cyan | `#00ACE9` |
| **Accent 2** | CLB Crimson | `#B41860` |
| **Accent 3** | CLB Rose | `#EB6898` |
| **Accent 4** | CLB Deep Blue | `#0068A9` |
| **Accent 5** | CLB Lime | `#96D654` |

### Functional colors

| Role | Hex | Notes |
|------|-----|-------|
| Text on dark backgrounds | `#FFFFFF` | |
| Muted / secondary text | `#767676` | WCAG AA compliant on white |
| Hyperlink | `#F59E00` | Amber |
| Visited link | `#B2B2B2` | |

### Usage rules
- **Navy dominates** (~60% visual weight), Cyan supports (~25%), Crimson punctuates (~15%).
- Lime (`#96D654`) is reserved for data callouts or positive indicators — not decoration.
- Do not introduce colors outside this palette.

---

## 3. Typography

### Font
**Century Gothic** — exclusively, for all text elements.

> HTML/CSS fallback stack: `'Century Gothic', 'Futura', 'Gill Sans', sans-serif`

### Type scale

| Element | Size | Weight | Color |
|---------|------|--------|-------|
| Presentation title | 40–44 pt | Bold | `#2C426C` or `#FFFFFF` |
| Slide title | 28–36 pt | Bold | `#2C426C` or `#FFFFFF` |
| Section header | 24–28 pt | Bold | `#2C426C` |
| Body text level 1 | 16–18 pt | Regular | `#2C426C` |
| Body text level 2 | 14 pt | Regular | `#2C426C` |
| Caption / footnote | 10–12 pt | Regular | `#767676` |
| Footer / URL | 8 pt | Regular | `#FFFFFF` or `#767676` |

### Rules
- Century Gothic only — no Arial, Calibri, or system defaults.
- Titles: left-aligned on content slides.
- Body text: always left-aligned. Never center body or bullet text.
- Line spacing: 1.15–1.3× body, 1.0× captions.
- No ALL CAPS except the "LÉON BÉRARD" logotype.

---

## 4. Layout Conventions

- **Margins:** minimum 0.7 cm from slide edges.
- **Body text left margin:** ~1.8 cm from left edge.
- **Title zone top margin:** ~1.2 cm from top.
- **Two-column layouts:** 50/50 split with ~0.5 cm gutter. Text always left-anchored; images right.
- **Logo:** top-right corner on every slide.
- **Footer URL** (`centreleonberard.fr`): bottom-right, 8 pt.
- **Date** (`DD/MM/YYYY`): bottom-left.

---

## 5. Component Patterns

### Bullet lists
- Level 1: navy dot, 16–18 pt Regular.
- Level 2: grey dash, 14 pt, indented ~0.6 cm.
- Maximum 2 nesting levels. Maximum ~6 bullets per slide.

### Tables
- Header row: `#2C426C` fill, white text, bold, 14 pt.
- Body rows: alternating white / `#F2F7FB`.
- Borders: 1 pt, `#0068A9`.
- Cell padding: 0.15 cm top/bottom, 0.3 cm left/right.

### Data callouts
- Number: 48–60 pt, bold, `#00ACE9` or `#2C426C`.
- Label: 14 pt Regular, `#2C426C`.

### Charts
- Color order for multi-series: Navy → Cyan → Crimson → Deep Blue → Lime → `#767676`.
- No 3D effects, no shadows.
- Legend: Century Gothic 12 pt, left-aligned or bottom.
- Grid lines: `#D9D9D9`, 0.5 pt.

---

## 6. HTML CSS Variables

```css
:root {
  /* Brand */
  --clb-navy:    #2C426C;
  --clb-cyan:    #00ACE9;
  --clb-crimson: #B41860;
  --clb-rose:    #EB6898;
  --clb-blue:    #0068A9;
  --clb-lime:    #96D654;

  /* Functional */
  --clb-white:   #FFFFFF;
  --clb-muted:   #767676;
  --clb-amber:   #F59E00;
  --clb-row-alt: #F2F7FB;

  /* Typography */
  --font: 'Century Gothic', 'Futura', 'Gill Sans', sans-serif;

  /* Slide canvas (16:9 at 100px/inch) */
  --slide-w: 1333px;
  --slide-h:  750px;

  /* Spacing */
  --margin-edge: 28px;
  --margin-body: 72px;
  --gutter:      20px;
}

h1 { font-family: var(--font); font-size: 2.75rem; font-weight: 700; color: var(--clb-navy); }
h2 { font-family: var(--font); font-size: 2rem;    font-weight: 700; color: var(--clb-navy); }
p, li { font-family: var(--font); font-size: 1rem; font-weight: 400; color: var(--clb-navy); line-height: 1.3; }
.caption { font-size: 0.7rem; color: var(--clb-muted); }
```

---

## Quick Reference

```
Font:     Century Gothic (all weights)
Bg light: #FFFFFF   Bg dark: #2C426C
Text:     #2C426C on white / #FFFFFF on dark
Muted:    #767676
Cyan:     #00ACE9   Crimson: #B41860
Blue:     #0068A9   Lime:    #96D654 (data only)
Ratio:    16:9  |  Logo: top-right  |  Date: bottom-left DD/MM/YYYY
```
