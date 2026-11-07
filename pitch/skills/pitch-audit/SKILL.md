---
name: pitch-audit
description: Stage 2 of the pitch workflow. Maps the target company's users and jobs, walks their key flows in the browser, or reconstructs them from UI glimpses found online and from screenshots and recordings the user supplies when the product is gated, logs UI, UX, and AI-opportunity findings with severity and screenshots, and writes 02-audit.md, then stops for approval. Use when the user says "pitch audit", "audit <company>'s UX for the pitch", "stage 2", "I dropped the screenshots in", or runs /pitch next with stage 2 pending.
argument-hint: "[slug]"
---

Stage 2 of `/pitch`. Produces `02-audit.md` and `02-evidence/`. Runs the `pitch:pitch-auditor` agent. Two passes when the territory cannot be seen at all.

## Before anything

1. The plugin root is two directories above this skill's base directory. Read `<plugin-root>/references/workspace.md` and follow its approval protocol exactly.
2. Find the workspace (argument slug, the only pitch, or ask).
3. Precondition: stage 1 `approved`. If stage 2 is `needs-access`, this is the second pass: check that `02-evidence/inbox/` has files; if it is empty, show `02-access-request.md` again and stop.
4. Set stage 2 to `running`.

## The brief for the agent

Spawn `pitch:pitch-auditor` (fallback `pitch-auditor`) in the foreground:

```
Workspace: <abs path>
Read first: 00-brief.md, 01-context.md (the Territory section drives flow priority), 01-glimpses/index.md
Mode: <probe | ingest>   (ingest when 02-evidence/inbox/ has files)
References: <plugin-root>/references/rubric.md, skill-map.md (stage 2), workspace.md (digest, rules), access-request.md
Template: <plugin-root>/references/templates/02-audit.md
Write: 02-audit.md, screenshots into 02-evidence/, and 02-access-request.md only if a territory flow is blocked and has no glimpses
Revision notes: <none, or the user's notes verbatim>
```

## After it returns

If the agent reports `needs-access`: set stage 2 to `needs-access`, present the digest, then the access request's flow table, say where to drop files (`02-evidence/inbox/`, screenshots preferred, recordings need ffmpeg), and end the turn. `/pitch next` resumes.

Otherwise follow steps 3 to 5 of the approval protocol. Present the digest in full, then the top ten with their evidence refs, then the AI opportunities. Say plainly which flows were walked live and which were reconstructed from glimpses.

On Approve, invoke `pitch-system` with the slug in the same turn. Stage 4 follows stage 3.

The agent never creates accounts, enters credentials, or submits forms with personal data on the company's site. If a free signup would unlock the flows, it asks the user through the access request instead.
