---
name: pitch-system
description: Stage 3 of the pitch workflow. Finds the target company's published design system or reverse-engineers one from their live site, brand guidelines, and every screenshot of the product found online or supplied: colours, type, spacing, radii, shadows, components, copy voice. Writes DESIGN.md, PRODUCT.md, mood.md, voice.md, tokens.md, tokens.css, inventory.md, DIGEST.md into 03-design-system/ so the prototype can feel like it came from inside their team even when the product is behind a login, then stops for approval. Use when the user says "pitch system", "extract <company>'s design system", "pull their tokens", "stage 3", or runs /pitch next with stage 3 pending.
argument-hint: "[slug]"
---

Stage 3 of `/pitch`. Produces `03-design-system/`. Runs the `pitch:pitch-extractor` agent.

## Before anything

1. The plugin root is two directories above this skill's base directory. Read `<plugin-root>/references/workspace.md` and follow its approval protocol exactly.
2. Find the workspace.
3. Precondition: stage 2 `approved`.
4. Set stage 3 to `running`.

## The brief for the agent

Spawn `pitch:pitch-extractor` (fallback `pitch-extractor`) in the foreground:

```
Workspace: <abs path>
Read first: 01-context.md (Design and brand guidelines found; UI glimpses), 01-glimpses/index.md and its images, 02-audit.md, the screenshots in 02-evidence/
Gated: <yes | no>  (yes when stage 2 could not walk the product live; extraction then leans on the marketing site, brand pages, login page, and screenshots)
References: <plugin-root>/references/design-md.md (the output contract), skill-map.md (stage 3), workspace.md (digest, rules)
Templates: <plugin-root>/references/templates/03-DIGEST.md
Write: 03-design-system/DIGEST.md, DESIGN.md, PRODUCT.md, mood.md, voice.md, tokens.md, tokens.css, inventory.md, raw/
Figma push: no   (set yes only if the user asked for tokens in Figma)
Revision notes: <none, or the user's notes verbatim>
```

## After it returns

Follow steps 3 to 5 of the approval protocol. Present the digest in full, including the per-category confidence. Then the three rules from `DESIGN.md`'s Do's and Don'ts that most define their look, and the token table's low-confidence rows by name. Offer `DESIGN.md` in the file pane.

On Approve, invoke `pitch-concepts` with the slug in the same turn if stage 4 is still pending; otherwise invoke `pitch-build`.

The user said the output has to feel like it came from inside the company's team. Low-confidence tokens are worse than missing ones, because the builder will trust them. If many values are measured from screenshots or guessed, say so plainly before asking for approval.
