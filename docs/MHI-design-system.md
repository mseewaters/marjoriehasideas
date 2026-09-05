# Marjorie Has Ideas — Visual Design System

## Brand Character

Marjorie Has Ideas (MHI) is a personal collection of projects, experiments,
ideas, and things Marjorie built while she was supposed to be doing something else.

The visual identity should feel:

- Personal
- Curious
- Well-traveled
- Editorial
- Slightly vintage
- Intelligent without being corporate
- Handmade without being crafty
- Playful without being cute
- Polished, but deliberately imperfect

Visual inspiration:
- Passport stamps
- Travel journals
- Old correspondence
- Typewritten notes
- Editorial print layouts
- Postmarks and field notebooks

Avoid:
- Generic SaaS aesthetics
- Gradients
- Neon colors
- Tech-startup blue
- Excessively rounded UI
- Glassmorphism
- Generic AI-generated illustrations
- Overly cute scrapbook aesthetics
- Excessive decorative travel imagery

---

# Color Palette

## Primary — Mulberry

`#4B1F4F`

Primary MHI brand color.

Use for:
- MHI logo/stamp
- Major headings
- Important accents
- Selected navigation states
- Links where appropriate
- Brand moments

This should be the color most strongly associated with MHI.

---

## Secondary — Winter Berry

`#B8326B`

Use sparingly for:
- Small accents
- Highlights
- Interactive states
- Occasional project/category details

Do NOT allow this to compete with Mulberry as the primary brand color.

---

## Accent — Inkwell Navy

`#1B2A6B`

Use for:
- Secondary accents
- Links
- Small graphic elements
- Postmark/travel-journal details

Should feel like blue fountain-pen ink.

---

## Neutral — Charcoal

`#3B3B45`

Primary body text and neutral UI color.

Use instead of pure black for most typography.

---

## Background — Parchment

`#F4EFE7`

Primary site background.

This is intentionally NOT pure white.

The site should retain a subtle paper/editorial feeling.

---

## Supporting White

`#FFFDFC`

Use only when additional contrast from Parchment is required, such as cards
or content surfaces.

Avoid large expanses of stark white.

---

# Recommended CSS Variables

```css
:root {
  --mhi-mulberry: #4B1F4F;
  --mhi-winter-berry: #B8326B;
  --mhi-inkwell: #1B2A6B;
  --mhi-charcoal: #3B3B45;
  --mhi-parchment: #F4EFE7;
  --mhi-white: #FFFDFC;

  --color-background: var(--mhi-parchment);
  --color-text: var(--mhi-charcoal);
  --color-primary: var(--mhi-mulberry);
  --color-secondary: var(--mhi-winter-berry);
  --color-accent: var(--mhi-inkwell);
}