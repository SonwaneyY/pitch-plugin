# pitch

Apply to a design role by building something for the company instead of sending a resume.

One run is one company and one role. Six stages, each run by its own sub-agent. After each stage you see the full digest of what was found, what was learned, and what went wrong, then approve. On approval the next stage starts at once, so one `/pitch <company>` carries through to a hosted prototype and a note, stopping only when you say Revise or Stop, or when the audit needs screenshots from you.

| Stage | Skill | Agent | Output |
|---|---|---|---|
| 0 Intake | `/pitch <company>` | none | `00-brief.md`, `STATUS.md` |
| 1 Context | `/pitch-context` | `pitch-researcher` | `01-context.md` (business, offerings inventory, users' voice, hiring manager, role), `01-glimpses/` (every UI image found online) |
| 2 UX audit | `/pitch-audit` | `pitch-auditor` | `02-audit.md`, `02-evidence/` |
| 3 Design system | `/pitch-system` | `pitch-extractor` | `03-design-system/` (DESIGN.md, PRODUCT.md, mood, voice, tokens, inventory) |
| 4 Concepts | `/pitch-concepts` | `pitch-strategist` | `04-concepts.md` (futures work, fixes, improvements, new offerings, overlap check, shortlist), `04-brief.md` |
| 5 Build | `/pitch-build` | `pitch-builder` | `05-prototype/` with `WALKTHROUGH.md` (click by click) and `NOTES.md` |
| 6 Ship | `/pitch-ship` | `pitch-writer` | `06-thesis.md`, `06-note.md`, hosted link |

`/pitch next` resumes after a Stop or in a new session. `/pitch status` shows every pitch in the current folder.

Outreach and tracking are out of scope. This plugin ends at a hosted prototype, a thesis, and a short note.

## What it is built to get right

- The research inventories everything the company already offers, and stage 4 checks every concept against it. A concept that already exists is excluded and named in the digest.
- Stage 1 hunts for every glimpse of the UI that exists online (demo videos, help center, app stores, review galleries, press kits) so stages 2 and 3 work when the product is behind a login.
- Stage 3 writes a `DESIGN.md` in the Google Labs format from live CSS where reachable and from measured screenshots where not, with confidence per value, so the prototype feels like the company's own.
- Stage 4 runs futures work with `product-strategy-session` and produces fixes, improvements, and new offerings, not only problems.
- Stage 5 builds on the extracted tokens with `impeccable`, self-critiques with `visual-critique`, then walks you through every click.
- Stage 6 states the thesis (idea, evidence, how the prototype answers it) before any note is written.

## What it reuses

Each stage invokes installed skills from the designer-skills marketplace, the PM toolkit, impeccable, and the Figma plugin where they exist. See `references/skill-map.md` for the list and the fallback when one is missing.

## Requirements

- Claude Code with the built-in browser (for live UX audit, UI glimpse capture, and screenshots).
- Optional: Figma plugin authorized (prototype to Figma, recordings to storyboard).
- Optional: `ffmpeg` on PATH (ingest screen recordings when the UI is gated).

## Install

From GitHub:

```
claude plugin marketplace add SonwaneyY/pitch-plugin
claude plugin install pitch@pitch-plugin
```

From a local clone:

```
claude plugin marketplace add <path-to-clone>
claude plugin install pitch@pitch-plugin
```

After editing a local clone, run `claude plugin update pitch@pitch-plugin` so the cached copy matches.
