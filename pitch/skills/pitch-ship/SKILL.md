---
name: pitch-ship
description: Stage 6 of the pitch workflow. First lays out the thesis behind the prototype for the user (the idea, the problem or opportunity and the evidence it came from, how the prototype answers it), then after confirmation writes the short conversational note to the hiring manager, de-slops it, and after approval hosts the prototype as a private shareable link. Use when the user says "pitch ship", "what's the thesis", "write the note for <company>", "host the prototype", "stage 6", or runs /pitch next with stage 6 pending.
argument-hint: "[slug]"
---

Stage 6 of `/pitch`. Produces `06-thesis.md`, `06-note.md`, and `06-link.md`. Runs the `pitch:pitch-writer` agent twice, thesis then note; the main thread hosts the prototype after the note is approved.

## Before anything

1. The plugin root is two directories above this skill's base directory. Read `<plugin-root>/references/workspace.md` and follow its approval protocol exactly.
2. Find the workspace.
3. Precondition: stage 5 `approved`.
4. Set stage 6 to `running`.

## Step one: the thesis

The applicant asked that, before any outreach is drafted, they first see exactly what the prototype was built on: the idea, the problem found through reviews or the audit, and how the prototype fulfils or solves it. Spawn `pitch:pitch-writer` (fallback `pitch-writer`) in the foreground:

```
Workspace: <abs path>
Step: thesis
Read first: 00-brief.md, 01-context.md (users' voice, business lines, hiring manager), 02-audit.md (findings by ID), 04-concepts.md, 04-brief.md, 05-prototype/NOTES.md, 05-prototype/WALKTHROUGH.md
References: <plugin-root>/references/rubric.md (Delivering it, What fails), skill-map.md (stage 6), workspace.md (digest, rules)
Template: <plugin-root>/references/templates/06-thesis.md
Write: 06-thesis.md
Revision notes: <none, or the user's notes verbatim>
```

When it returns, set stage 6 to `draft` and present `06-thesis.md` in full: the digest, the idea, the problem or opportunity with its evidence trail, who it is for, the prototype mapped to the problem step by step, what it does not solve, the measure, the one-line version. Ask "Is this the thesis?" with Approve, Revise, Stop. Revise goes back to the same agent with the notes.

## Step two: the note

On thesis approval, send the same agent:

```
Step: note. The thesis in 06-thesis.md is confirmed. Write 06-note.md from <plugin-root>/references/templates/06-note.md.
```

When it returns, present the note itself in full (it is short), then its digest, then the "if they ask" bullets. Ask "Note on track?" with Approve, Revise, Stop. Revise rounds go to the same agent.

## On note approval: host

1. Read the artifact `guide` (topic `pages`) if this session has not yet, and check `05-prototype/index.html` against it: a short `<title>`, viewport meta, external scripts only from the allowed CDNs, total size under the limit. Fix trivial violations in place; anything larger goes back to stage 5 as a revision.
2. Publish `05-prototype/` with `publish_files`: `index.html` plus every file it references, each at its relative path. Artifacts are private by default; the user decides who gets the link.
3. Write `06-link.md` with the URL and the date. Replace the bracketed link placeholder in `06-note.md` with the URL.
4. Set stage 6 `approved`. Tell the user the link, the note path, the thesis path, and that outreach is theirs from here. Offer the Figma push (`figma:figma-code-workflows`, `html_to_figma`) if they have not had it yet and the Figma MCP is authorized.

This is the last stage. Nothing chains after it.

Never send the note anywhere. Never draft it in a mail client. The user asked for outreach to be a separate workflow.
