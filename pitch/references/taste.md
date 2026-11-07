# Taste floor

The user's hard constraint: the prototype must not look AI-generated. It has to be tasteful and feel like the company's own team made it. The user will take it into Figma afterwards, so it is a strong starting point, not a final file; but a generic starting point wastes their time.

impeccable's `reference/craft-floor.md` is the full version of this list and the builder loads it before any UI edit. This page is the short form, for when impeccable is unavailable and for the self-check before showing the user.

## Build on their system, not a generic one

- Every colour, font, radius, spacing step, and shadow comes from `DESIGN.md`. A value that is not in the tokens needs a sentence in `NOTES.md` explaining why.
- Copy sounds like `voice.md`. Buttons name their action in their words. Errors name the problem and the way out.
- Icons come from a real set in one stroke weight, ideally the one they use. Never emoji, never unicode glyphs standing in for icons.
- Content is shaped like their content: real-looking company names, job titles, numbers with their formatting. No lorem ipsum, no "John Doe".

## Defaults that read as AI-made (avoid unless their UI does it)

- A kicker or eyebrow label above a heading.
- Same-size cards of icon plus heading plus text as the page structure. Nested cards.
- The hero-metric block: big number, small label, supporting stats.
- Gradient text. Glass and blur as decoration.
- A thick coloured left border on cards, callouts, or alerts.
- Hard offset block shadows outside a neobrutalist world.
- Sparklines and progress rings standing in for content.
- Monospace as a costume for "technical".
- Section numbers (01, 02, 03) when the order carries no information.
- One identical fade-up entrance on every section.
- A modal for something that needs no interruption.

## Things that make it look built, not assembled

- Themed selection colour, caret, focus ring, scrollbar, underline offset, tabular numerals.
- Hover, active, disabled, loading, empty, and error states present on the interactive pieces.
- Spacing that reads as groups: tight inside, generous between, more above a heading than below.
- Body text measure around 65 to 75 characters. Contrast 4.5:1 for body text.
- At most one authored motion moment, ease-out, from an already-visible state.
- Real responsive behaviour at phone width.

## The detail worth the human's hand

The articles are explicit: the signal is one small detail an AI would not have sweated. The builder does not try to be that detail. It builds the slice well, then writes two or three candidates into `NOTES.md` under "Worth your hand", each with why it matters and what finishing it would involve. The user picks one and finishes it in Figma or by prompting.
