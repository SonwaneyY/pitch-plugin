---
name: pitch
description: Start or continue a spearfishing job application for one company and one design role: research the company and what it already offers, audit their UX, extract their design system, run futures work and pick one concept, build an on-brand prototype, write the thesis and the note. Use whenever the user wants to apply to a design role by building something for the company instead of sending a resume, says "pitch <company>", "spearfish", "start a pitch for", "pitch next", "pitch status", or "what stage is the <company> pitch at". Also use when the user names a company and a hiring manager and wants a prototype with ideas for them.
argument-hint: "<company> [role url or title] | next [slug] | status"
---

Runner for the pitch workflow. Six stages, one sub-agent each. After each stage the applicant sees the full digest and approves; on approval the next stage starts in the same turn, so one `/pitch <company>` carries through to the hosted link with no re-invocation, stopping only when the applicant says Revise or Stop or when the audit needs files from them. This skill handles intake, `status`, and `next`. The stage skills (`pitch-context`, `pitch-audit`, `pitch-system`, `pitch-concepts`, `pitch-build`, `pitch-ship`) do the work.

The plugin root is two directories above this skill's base directory. Read `<plugin-root>/references/workspace.md` first; it defines the folder layout, `STATUS.md`, the stage order, the digest, and the approval protocol every stage follows.

## `status`

Find every `pitches/*/STATUS.md` under the current directory. Print one line per pitch: company, role, the stage table collapsed to `1 approved, 2 draft, 3 pending ...`, and the next command. If none exist, say so and show the intake form of arguments.

## `next [slug]`

Resumes a pitch after a Stop, an access request, or in a new session.

1. Pick the workspace: the slug if given, the only pitch if there is one, otherwise ask which with AskUserQuestion.
2. Read its `STATUS.md`. The next stage is the first row whose status is not `approved`, following the order table in `workspace.md`.
3. Invoke that stage's skill with the Skill tool, passing the slug as the argument. The stage skill owns the approval and the hand-off to the stage after it.

## Intake: `<company> [role]`

One pitch is one company and one role. Collect what the stages need, in one AskUserQuestion round where possible, and do not research anything yet.

Required:
- Company name and URL.
- The role: a job posting URL, or a title plus the pasted description.

Optional, ask once, accept "skip":
- Hiring manager name. Name only. Their email is not needed by any stage and is not stored.
- A path to a resume or portfolio notes.
- The applicant's angle in one or two sentences: why this company, what they want to be known for. This feeds concept scoring and the note, so an honest line beats a polished one.
- Constraints: time budget (default a weekend), platform (default web), anything off limits.

Then:
1. Make the slug (company lower-cased, non-alphanumerics to `-`). If `pitches/<slug>/` exists, say so and offer `next` instead of overwriting.
2. Create the folder tree from `workspace.md`, including `01-glimpses/` and `02-evidence/inbox/`.
3. Write `00-brief.md` from `<plugin-root>/references/templates/00-brief.md` with the answers. If a role URL was given, fetch it (WebFetch, or the `web-fetch` agent) and paste the description into the brief; if the fetch fails, say so and ask for a paste.
4. Write `STATUS.md` from the block in `workspace.md`, with the absolute workspace path and today's date.
5. Show the brief in five lines, then invoke `pitch-context` with the slug. The run carries on from there, one approval per stage.

## Scope reminders

- Outreach and tracking are not part of this plugin. Never send anything, never draft an email in a mail client. The note in stage 6 is a file.
- The approval after each stage is the gate the applicant asked for. Never skip it, never pre-answer it. On Approve, continue; on anything else, stop.
