---
name: pitch-concepts
description: Stage 4 of the pitch workflow. Runs futures work on the research with product-strategy-session, turns the audit and the futures into six concepts of three kinds (fixes, improvements, new offerings), checks each against what the company already offers and excludes overlaps, scores the rest, shortlists two or three, lets the user pick one, then writes the design brief for the pick and stops for approval. Use when the user says "pitch concepts", "what should I build for <company>", "ideas for the pitch", "stage 4", or runs /pitch next with stage 4 pending.
argument-hint: "[slug]"
---

Stage 4 of `/pitch`. Produces `04-concepts.md`, then `04-brief.md` after the user picks. Runs the `pitch:pitch-strategist` agent. Two approvals: the shortlist and pick, then the brief.

## Before anything

1. The plugin root is two directories above this skill's base directory. Read `<plugin-root>/references/workspace.md` and follow its approval protocol exactly.
2. Find the workspace.
3. Precondition: stage 2 `approved`. Stage 3 is not required; if `03-design-system/` exists the agent reads it for feasibility.
4. Set stage 4 to `running`.

## The brief for the agent

Spawn `pitch:pitch-strategist` (fallback `pitch-strategist`) in the foreground:

```
Workspace: <abs path>
Read first: 00-brief.md (angle, constraints), 01-context.md (offerings inventory, strategy and direction, territory, hiring manager priorities, users' voice), 02-audit.md (findings, top ten, AI opportunities), 03-design-system/ if present
References: <plugin-root>/references/rubric.md (picking the problem, sizing), skill-map.md (stage 4), workspace.md (digest, rules)
Templates: <plugin-root>/references/templates/04-concepts.md, 04-brief.md
Write now: 04-concepts.md only
Revision notes: <none, or the user's notes verbatim>
```

## After it returns: the pick

1. Set stage 4 to `draft`. Present the digest in full. Its What went wrong section lists every concept excluded for already existing; read those out, with the offering and source, because the applicant asked to be told exactly when that happens. Then the Futures work section. Then the shortlist: for each concept, its kind, the problem or opportunity in one line, the slice in one line, its overlap result, its total. Then the recommendation in one sentence.
2. Ask with AskUserQuestion which concept to build. Options are the shortlisted concepts, recommendation first, plus "Revise the concepts" and "Stop".
   Revise: send the notes to the same agent with SendMessage, return to step 1.
   Stop: leave `draft`.
3. On a pick: send the agent `User picked <C#>. Write 04-brief.md from the template, using ux-strategy:frame-problem for the brief, success metrics, and principles. Carry the evidence pack and the detail worth sweating.` Wait for it.
4. Present the brief's digest, then the slice, the success metric, the principles, and the detail worth sweating. Ask "Brief on track?" with Approve, Revise, Stop, per the protocol.
5. On Approve: set stage 4 `approved`, note the picked concept in the STATUS.md notes column. Then, in the same turn, invoke `pitch-build` with the slug if stage 3 is approved; otherwise invoke `pitch-system`.

The rubric says a full redesign reads as presumptuous and a slice of one flow does not. If the user picks a concept whose slice is more than three screens, say so once, then respect the pick.
