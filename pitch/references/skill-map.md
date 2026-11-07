# Which installed skills each stage invokes

Invoke these with the Skill tool, by the exact name shown. They come from the designer-skills marketplace, the PM toolkit in the user's skills folder, impeccable, and the Figma plugin. If a name is not in the available-skills list, use the fallback and note the gap in your report; do not invent the skill's method from its name.

A skill marked **interactive** runs a multi-turn workshop and cannot be used by a sub-agent. They are listed so nobody tries.

## Stage 1 Context (pitch-researcher)

| Skill | Use it for |
|---|---|
| `ux-strategy:business-design` | the lens: revenue and cost drivers, which line a design change moves, competitive map by experience not features |
| `ux-strategy:competitive-analysis` | direct, indirect, aspirational competitors; task-level comparison |
| `anthropic-skills:deep-research` | the research engine for launches, strategy, AI moves, recent news |
| `positioning-statement` | state their positioning in one Moore-format sentence |
| `last30days` | what users say in the last 30 days on Reddit, X, HN, YouTube. Optional; needs its Python engine and keys. Skip silently if it fails. |

| `web-fetch` agent | stands in for WebFetch when the session has no WebFetch tool; pass the URL and the question |

Not used: `business-health-diagnostic` (interactive), `tam-sam-som-calculator`.
No skill covers: the offerings inventory sweep, the UI glimpse hunt (YouTube, help center, app stores, review galleries, press kits, design galleries, Wayback Machine), brand guideline discovery, hiring manager research. Methods are in the agent prompt; the tools are WebSearch, page fetches, and the browser pane with screenshots of video players at UI moments.

Fallback: WebSearch plus page fetches, structured by the `01-context.md` template.

## Stage 2 UX audit (pitch-auditor)

| Skill | Use it for |
|---|---|
| `design-research:discover` | chains persona, empathy map, journey map: who the users are and what they go through |
| `jobs-to-be-done` | the user's own copy, component type. Turn users into tasks to attempt. |
| `prototyping-testing:heuristic-evaluation` | the walkthrough and the finding format: heuristic, location, severity 0 to 4, screenshot, recommendation |
| `prototyping-testing:user-flow-diagram` | document each flow as attempted |
| `design-ops:design-debt-audit` | visual inconsistency and structural debt; severity times frequency over effort |
| `visual-critique:critique-ux` | hierarchy, affordance, density on each key screenshot |
| `evaluation:heuristic-evaluation-ai` | the AI-opportunity lens |
| `model-interaction-design:audit-interaction` | only if they already ship AI features |
| `cognitive-accessibility:assess-load` | optional load map on a multi-step flow |
| `figma:video-interaction-mapper` | ingest a screen recording into key frames. Needs ffmpeg, Pillow, Figma auth. |

Not used: `break-ui`, `agentation` (built for your own code).

Fallback: Nielsen's ten heuristics from memory, severity 0 to 4, one screenshot per finding.

## Stage 3 Design system (pitch-extractor)

| Skill | Use it for |
|---|---|
| `impeccable` (`document` reference) | the DESIGN.md format only. The command itself reads project code, not a live site. |
| `design-systems:tokenize` | runs on a CSS file: dedupe, tiers, naming. Feed it the stylesheets you pulled. |
| `design-systems:design-token` | token taxonomy and naming |
| `ui-design:typography-scale`, `ui-design:spacing-system`, `ui-design:color-system` | structure the extracted values |
| `designer-toolkit:ux-writing`, `cognitive-accessibility:plain-language-design` | describe the copy voice for `voice.md` |
| `figma:figma-generate-library` | optional: push tokens to Figma as variables. Needs auth. |

Live extraction (computed styles, custom properties, fonts, radii, shadows from the running site) has no skill. Method is in the agent prompt.

Fallback: write DESIGN.md from screenshots and visible CSS by hand, following `references/design-md.md`.

## Stage 4 Concepts (pitch-strategist)

| Skill | Use it for |
|---|---|
| `product-strategy-session` | the futures work. Workflow type; run it in context-dump mode with the stage 1 and 2 evidence as the argument, through Phase 4. When it or a chained skill poses a question, the agent answers from the evidence with a citation. Where it names an interactive skill, apply that framework's structure directly. |
| `press-release` | one working-backwards paragraph per new-offering concept; a weak paragraph means a weak concept |
| `pestel-analysis` | market forces for the futures section, only when the market is moving in ways that bear on the product |
| `proto-persona`, `jobs-to-be-done`, `positioning-statement` | the component skills the strategy session reaches for; all run non-interactively |
| `ux-strategy:opportunity-framework` | the scoring: impact-effort, RICE, value vs complexity |
| `problem-statement` | one per shortlisted concept |
| `epic-hypothesis` | a testable hypothesis per concept |
| `recommendation-canvas` | AI-opportunity concepts only |
| `ux-strategy:business-design` | which line each concept moves |
| `ux-strategy:frame-problem` | after the pick: design brief, success metrics, principles |
| WebSearch | the overlap check: concept idea plus company name, to catch what the offerings inventory missed |

Interactive, never run as workshops inside the agent: `opportunity-solution-tree`, `prioritization-advisor`, `problem-framing-canvas`, `lean-ux-canvas`, `positioning-workshop`. When the strategy session reaches one, the agent applies its framework (OST: outcome, opportunities, solutions; problem framing canvas; RICE or value-effort) directly from the evidence and moves on.

## Stage 5 Build (pitch-builder)

| Skill | Use it for |
|---|---|
| `impeccable` | the build engine. Reads DESIGN.md and PRODUCT.md from the prototype root. Mode Operate. Its craft-floor reference is the taste floor. |
| `emil-design-eng` | detail craft: states, motion decisions, invisible correctness |
| `prototyping-testing:user-flow-diagram`, `interaction-design:map-states` | flow and states before any markup |
| `prototype` | three divergent working versions of one piece behind a picker. Use for the key screen or the detail worth sweating. Explicit invoke only. |
| `animate` | the one authored motion moment, if the concept earns it |
| `break-ui` | worst-case data pass on the prototype before showing the user |
| `visual-critique:critique-screen` | the self-critique gate against mood.md, voice.md, tokens.md |
| `impeccable polish`, `clarify`, `harden` | final pass, copy, edge states |
| `figma:figma-code-workflows` | `html_to_figma`: static HTML to editable Figma layers, the handoff to the user's Figma. Needs auth. |
| `figma:figma-design-to-code` | reverse mode when the user designed in Figma first |
| `designer-toolkit:ux-writing` | microcopy in the company's voice |

Not used: `ui-design:design-screen` (weaker than impeccable), `mobile-native`, `pick-ui-library` unless the build needs them.

Fallback: plain HTML and CSS on `tokens.css`, then `references/taste.md` as the checklist.

## Stage 6 Ship (pitch-writer, then main thread)

The writer runs twice: the thesis (no skill; the agent traces evidence to prototype from the stage files), then the note.

| Skill | Use it for |
|---|---|
| `designer-toolkit:design-rationale` | the note's skeleton: decision, context, options, evidence, reasoning, trade-offs, validation |
| `copy-editing` | the de-slop pass; it strips AI tells by rule |
| `ux-strategy:metrics-definition` | the "how I would measure it" line |
| the artifact `guide` (topic `pages`) plus `publish_files` | hosting, done in the main thread after the note is approved |

Not used: `case-study` (the user wants a paragraph, not a page).

## Duplicates resolved

- Jobs to be done: the user's `jobs-to-be-done` over `design-research:jobs-to-be-done`.
- Heuristics: `prototyping-testing:heuristic-evaluation` for their UI in stage 2; `impeccable critique` for ours in stage 5.
- Variants: `prototype` over `impeccable generate` (no live browser session needed).

## Known quirks

- `emil-design-eng`, `prototype`, `break-ui` answer with a one-line greeting when invoked without a task. Always pass the task in the invocation.
- `impeccable` runs a launcher (`impeccable context`) and expects `PRODUCT.md` and `DESIGN.md` at the project root. The prototype folder is that root. On Windows the launcher is `impeccable.cmd`.
- Figma skills need the Figma MCP authorized. If a Figma call fails for auth, note it and continue without Figma.
