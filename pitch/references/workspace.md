# Workspace layout and the approval protocol

Every pitch lives in its own folder under `pitches/` in the directory where `/pitch` was first run. Stages talk to each other through files in that folder, never through shared conversation context. A stage reads the files before it and writes its own; nothing else.

## Layout

```
pitches/<slug>/
  STATUS.md                 stage table, approvals, workspace path
  00-brief.md               intake: company, role, hiring manager name, your angle
  01-context.md             stage 1 output: business, offerings inventory, users' voice, hiring manager, role, territory
  01-glimpses/              stage 1 output: every screenshot or frame of their UI found online
    index.md                one row per file: source URL, date, what it shows, confidence it is current
  02-audit.md               stage 2 output
  02-access-request.md      only when the UI is gated; lists what the agent needs from you
  02-evidence/
    inbox/                  you drop screenshots or recordings here
    NN-<flow>-<step>.png    agent screenshots
  03-design-system/
    DIGEST.md               stage 3 digest
    DESIGN.md               tokens + rules, Google Labs DESIGN.md format (impeccable reads this)
    PRODUCT.md              what the product is, who uses it, mode (impeccable reads this)
    mood.md  voice.md  tokens.md   (visual-critique reads these three)
    tokens.css              extracted custom properties, usable as-is
    inventory.md            component inventory with evidence refs
  04-concepts.md            fixes, improvements, new offerings; scores; overlap check; shortlist; your pick
  04-brief.md               design brief for the picked concept
  05-prototype/             a self-contained static site; impeccable treats this as project root
    index.html ...
    WALKTHROUGH.md          click-by-click tour of the prototype
    NOTES.md                digest, details worth your hand, known gaps, what was left generic
  06-thesis.md              the idea, the problem and its evidence, how the prototype answers it
  06-note.md                the conversational paragraph
  06-link.md                hosted URL
```

The slug is the company name lower-cased, non-alphanumerics replaced by `-`.

## STATUS.md

```markdown
# Pitch: <Company>

company: <Company>
slug: <slug>
role: <title>
role_url: <url or "pasted">
hiring_manager: <name or "unknown">
workspace: <absolute path>
created: <YYYY-MM-DD>

| stage | file | status | approved_on | notes |
|---|---|---|---|---|
| 1 context | 01-context.md | pending | | |
| 2 audit | 02-audit.md | pending | | |
| 3 system | 03-design-system/ | pending | | |
| 4 concepts | 04-concepts.md | pending | | |
| 5 build | 05-prototype/ | pending | | |
| 6 ship | 06-note.md | pending | | |
```

Status values: `pending`, `running`, `draft`, `needs-access`, `revise`, `approved`.

## Preconditions and order

| Stage | Requires approved | Runs next on approve |
|---|---|---|
| 1 | nothing (needs `00-brief.md`) | 2 |
| 2 | 1 | 3, then 4 |
| 3 | 2 | 4 if pending, else 5 |
| 4 | 2 | 5 if 3 approved, else 3 |
| 5 | 3 and 4 | 6 |
| 6 | 5 | nothing |

## The digest

Every stage output file starts with a `## Digest` section, and the agent's final report is that digest verbatim. The applicant said they need to see exactly what was found and learned after every round, so the digest is the deliverable the main thread shows, not a summary of it. Sections, always in this order:

- **What I found.** The concrete findings with evidence refs. Complete, not the top three.
- **What I learned.** What changed in the picture since the previous stage. What surprised.
- **What went wrong.** Anything not done, not reachable, guessed, low confidence, or a rule bent. Named exactly, not softened. For stage 4 this includes every concept excluded because the company already offers it.
- **Confidence.** High, medium, or low, with the reason.
- **Open questions.** For the applicant.

Length is whatever the content needs. Do not trim to fit a line budget.

## The approval protocol (every stage skill follows this)

Sub-agents cannot talk to the user, so the gate lives in the main thread between stages. The applicant wants two things that hold at once: a real stop after every round where they see everything that was found, and no manual re-invocation between rounds. So: present everything, ask once, and on approval go straight into the next stage.

1. Read `STATUS.md`. Check the precondition. If unmet, say which stage is missing and stop.
2. Set this stage to `running`. Spawn the stage's agent in the foreground with the brief from the skill. Wait for it.
3. When it returns, set the stage to `draft` (or `needs-access` if the agent reported that). Present the Digest from the output file in full, every section, then the stage's own extras (the shortlist in stage 4, the walkthrough in stage 5, the thesis in stage 6). Offer the output file in the file pane.
4. Ask one question with AskUserQuestion: "Stage N on track?" Options: Approve, Revise, Stop. Revise takes notes.
5. Approve: set `approved` and today's date. Then, in the same turn, invoke the next stage's skill with the Skill tool, per the order table. Do not ask whether to continue; the approval was the answer.
   Revise: send the notes to the same agent with SendMessage so it keeps its context, then return to step 3.
   Stop: leave `draft`. Say that `/pitch next` resumes.
   `needs-access`: show the request, end the turn. `/pitch next` resumes once files are in the inbox.

Stage 6 ends the run. There is nothing to chain after it.

## Rules for every agent

- Never ask the user anything. Write questions under Open questions in the digest. The main thread surfaces them.
- Every claim about the company carries a URL or a screenshot path. A finding without evidence goes under `## Unverified` or gets cut.
- Read the files from earlier stages before starting. They are the brief.
- Start the output file with the digest. End your run by returning the digest verbatim as your report.
- No outreach of any kind. Never email, message, post, or submit a form on the company's site beyond what a visitor does to use the product. Never create accounts or enter credentials.
