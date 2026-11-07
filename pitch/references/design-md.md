# The design-system contract between stage 3 and stage 5

Stage 3 writes these files into `03-design-system/`. Stage 5 copies them into `05-prototype/` before building. Two installed tools read them: impeccable reads `DESIGN.md` and `PRODUCT.md`; visual-critique reads `mood.md`, `voice.md`, `tokens.md`. Get the formats right and the taste tools do their job for free.

## DESIGN.md (Google Labs design.md spec, what impeccable reads)

YAML frontmatter with machine-readable tokens, then up to eight markdown sections in this order. Tokens are normative. Prose explains how to apply them. Omit a section rather than inventing rules for it.

```yaml
---
name: <Company>
description: <one line, what the product is>
colors:
  primary: "#..."
  neutral-bg: "#..."
  # one entry per extracted colour; keep the company's own names where they expose them
typography:
  display:
    fontFamily: "<their stack>"
    fontSize: "..."
    fontWeight: 600
    lineHeight: 1.1
    letterSpacing: "-0.01em"
  body:
    fontFamily: "..."
    fontSize: "..."
    fontWeight: 400
    lineHeight: 1.5
rounded:
  sm: "4px"
  md: "8px"
spacing:
  sm: "8px"
  md: "16px"
components:
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.neutral-bg}"
    rounded: "{rounded.sm}"
    padding: "..."
  button-primary-hover:
    backgroundColor: "{colors.primary-deep}"
---
```

Rules: token refs use `{path.to.token}`; components may reference primitives, primitives never reference each other. Component sub-tokens are limited to `backgroundColor`, `textColor`, `typography`, `rounded`, `padding`, `size`, `height`, `width`. Shadows, motion, and focus rings go in the prose sections. Scale keys are open-ended; use the company's own names if their CSS exposes them. Variants are sibling keys (`button-primary`, `button-primary-hover`).

Sections, in order:

1. `## Overview`: the product, its visual personality in three adjectives, what the UI is optimising for.
2. `## Colors`: roles, when each is used, contrast notes, what is never coloured.
3. `## Typography`: families, scale, where each step appears, numerals, truncation habits.
4. `## Layout`: grid, container widths, breakpoints, density, page scaffolds they actually use.
5. `## Elevation & Depth`: shadows, borders, how layers separate.
6. `## Shapes`: radii, border widths, form language.
7. `## Components`: the inventory with how each behaves (states, sizes), referencing `inventory.md` screenshots.
8. `## Do's and Don'ts`: what their UI never does, what it always does. This section is where "feels like it came from inside the team" lives.

Every value in the frontmatter was observed, not guessed. Where you could not observe a value (a hover state you never saw), say so in prose rather than inventing a token.

## PRODUCT.md (what impeccable reads for context)

Short. What the product is, who uses it, the primary jobs, the surface mode (almost always Operate for product UI; Persuade for marketing pages), platform (web, responsive, native), and the three things the company visibly cares about in its UI. Pull from `01-context.md` and `02-audit.md`.

## mood.md, voice.md, tokens.md (what visual-critique reads)

- `mood.md`: brand personality keywords, aesthetic direction, imagery and illustration style, iconography style, explicit do and don't examples with evidence refs.
- `voice.md`: tone (direct or warm, formal or casual), sentence length, how CTAs are phrased, how errors are phrased, vocabulary they use and avoid, capitalisation habits. Quote real strings from their UI as examples, with screenshot refs.
- `tokens.md`: the same tokens as DESIGN.md frontmatter, as a readable table: colour, spacing, radius, shadow, typography. One row per token with the value and where it was observed.

## tokens.css

The custom properties as the site exposes them, renamed only when the site has none. Usable by the prototype as-is.

## inventory.md

One row per component observed: name, screenshot path, variants seen, states seen, notes on behaviour. This is the evidence for the Components section.
