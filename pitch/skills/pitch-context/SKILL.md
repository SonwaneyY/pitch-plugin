---
name: pitch-context
description: Stage 1 of the pitch workflow. Researches the target company's business, a complete inventory of what they already offer, what they shipped recently, how they use AI, strategy and direction, competitors, what users complain about, every glimpse of their UI available online, brand guidelines, the hiring manager's public professional footprint, and what the role owns, then writes 01-context.md and 01-glimpses/ and stops for approval. Use when the user says "pitch context", "research <company> for the pitch", "stage 1", or runs /pitch next with stage 1 pending.
argument-hint: "[slug]"
---

Stage 1 of `/pitch`. Produces `01-context.md` and `01-glimpses/`. Runs the `pitch:pitch-researcher` agent.

## Before anything

1. The plugin root is two directories above this skill's base directory. Read `<plugin-root>/references/workspace.md` and follow its approval protocol exactly.
2. Find the workspace: `pitches/<slug>/` where slug is the argument, or the only pitch under `pitches/`, or ask which with AskUserQuestion if there are several.
3. Precondition: `00-brief.md` exists. If not, send the user to `/pitch <company>`.
4. Set stage 1 to `running` in `STATUS.md`.

## The brief for the agent

Spawn `pitch:pitch-researcher` with the Agent tool, in the foreground, with this brief and absolute paths filled in:

```
Workspace: <abs path to pitches/<slug>>
Read first: 00-brief.md
References: <plugin-root>/references/rubric.md, <plugin-root>/references/skill-map.md (stage 1 section), <plugin-root>/references/workspace.md (digest, rules for every agent)
Templates: <plugin-root>/references/templates/01-context.md, <plugin-root>/references/templates/01-glimpses-index.md
Write: 01-context.md, 01-glimpses/ (images plus index.md)
Revision notes: <none, or the user's notes verbatim>
```

If the agent type `pitch:pitch-researcher` is not available, try `pitch-researcher`. If neither exists, tell the user the plugin's agents did not load and stop.

## After it returns

Follow steps 3 to 5 of the approval protocol. Present the digest in full. After it, add two things the applicant asked to see every time: the offerings inventory row count and which sweeps were skipped (so they can judge whether the "already exists" check in stage 4 will hold), and the glimpse coverage (how many UI images were found, which surfaces have none). Then the three territory bullets.

On Approve, invoke `pitch-audit` with the slug in the same turn.

Stage 1 gets the most scrutiny: a misread business poisons every later stage, and a thin offerings inventory lets an existing feature through as a concept. If the digest's What went wrong names a skipped sweep, say so before asking for approval and suggest Revise with the gap named.
