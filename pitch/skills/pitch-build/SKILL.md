---
name: pitch-build
description: Stage 5 of the pitch workflow. Builds the picked concept as a working, static, self-contained prototype on the company's extracted tokens and voice, runs its own taste and edge-case checks, then walks the user through it click by click and flags the details worth their hand, and stops for approval. Use when the user says "pitch build", "build the prototype for <company>", "stage 5", "make the <concept> prototype", "walk me through the prototype", or runs /pitch next with stage 5 pending. Also use when the user has refined the prototype in Figma and wants it rebuilt from the Figma file.
argument-hint: "[slug] [--from-figma <figma url>]"
---

Stage 5 of `/pitch`. Produces `05-prototype/`. Runs the `pitch:pitch-builder` agent.

## Before anything

1. The plugin root is two directories above this skill's base directory. Read `<plugin-root>/references/workspace.md` and follow its approval protocol exactly.
2. Find the workspace.
3. Precondition: stages 3 and 4 `approved`. Both. The build without the design system is the generic AI prototype the whole workflow exists to avoid.
4. Set stage 5 to `running`.

## The brief for the agent

Spawn `pitch:pitch-builder` (fallback `pitch-builder`) in the foreground:

```
Workspace: <abs path>
Read first: 04-brief.md, 03-design-system/ (all files), the screenshots and glimpses named in the brief's evidence pack, 05-prototype/NOTES.md and WALKTHROUGH.md if this is a rebuild
References: <plugin-root>/references/taste.md, skill-map.md (stage 5), workspace.md (digest, rules)
Templates: <plugin-root>/references/templates/05-NOTES.md, 05-WALKTHROUGH.md
Mode: <build | rebuild-from-figma <url>>
Write: 05-prototype/ (index.html, assets, FLOW.md, WALKTHROUGH.md, NOTES.md, screenshots/)
Figma push: <no | yes>   (yes only if the user asked for the prototype in Figma; needs the Figma MCP authorized)
Revision notes: <none, or the user's notes verbatim>
```

## After it returns: the walkthrough

The applicant asked to be walked through the prototype step by step, especially the clicks, so they understand what is being presented before they judge it. So the order is: digest, then the tour, then the prototype itself, then the question.

1. Set stage 5 to `draft`. Present the digest from `NOTES.md` in full.
2. Present `WALKTHROUGH.md` step by step: the starting state, then each numbered step with its "click or do", "you see", and "notice" lines, in order, all of them. Do not compress steps together.
3. Open `05-prototype/index.html` in the browser pane. Drive the walkthrough live: for each step, find the control it names, click it, take a screenshot, and say in one line what just changed. If a control cannot be found by its label, say so, show the agent's screenshot for that step instead, and carry on. End at phone width for the last screen.
4. Read out the "Worth your hand" list from `NOTES.md` verbatim. Those are the details the applicant finishes in Figma; naming them is part of the deliverable.
5. Ask "Stage 5 on track?" with Approve, Revise, Stop, per the protocol.

Revise is the normal path here, often several rounds. Each round goes to the same agent with SendMessage so it keeps the build context, and the walkthrough is re-presented after each round. If the user says the prototype looks AI-generated, pass that verbatim; the agent knows to re-read `taste.md` and the craft floor and to look for the listed defaults.

On Approve, invoke `pitch-ship` with the slug in the same turn.

## Rebuild from Figma

When the argument has `--from-figma <url>`, the user refined the design in Figma and wants it rebuilt. Set mode to `rebuild-from-figma`. The agent loads `figma:figma-design-to-code` and uses `get_design_context` on that URL; the Figma MCP must be authorized. If it is not, say so and stop before spawning.
