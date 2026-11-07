# Access request (when the UI is gated)

Business and enterprise products often sit behind a login, a paywall, or a sales demo. The auditor's first pass probes access. When it cannot reach the flows it needs, it writes `02-access-request.md` from this template and reports `needs-access`. The main thread shows the request to the user and ends the turn. The user drops files into `02-evidence/inbox/` and reruns `/pitch-audit`; the second pass ingests them.

Screenshots are the easy path: the agent reads images directly. Screen recordings need `ffmpeg` for frame extraction (and the Figma plugin for the storyboard route); if neither is available, ask for screenshots instead.

```markdown
# Access request: <Company>

I could reach: <list of public surfaces seen, with screenshot paths>
I could not reach: <login wall / paywall / enterprise-only / app-only>

## What I need from you

Drop files into `02-evidence/inbox/`. Name them so I can match them to flows; the exact name does not matter, the flow and step do.

| # | Flow | Steps I need to see | Why it matters | Preferred |
|---|---|---|---|---|
| 1 | <e.g. Employer: post a job> | landing, form, preview, confirmation | core revenue flow for the role's team | screenshots per step |
| 2 | ... | ... | ... | short recording ok |

Also useful, if you have it:
- the empty state of <X>
- any error you have hit
- the mobile view of <Y>

When the files are in, run `/pitch-audit` again. I will pick up from here.
```

Rules for the agent writing it:
- Ask for the fewest flows that cover the role's territory and the business's core loop. Five is a lot.
- Name the step boundaries, so the user knows when to screenshot.
- Never ask for credentials. Never ask the user to share an account.
