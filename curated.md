# Skills others built

Skill collections we have used and vouch for. Each entry has a source, license and install line.

## Anthropic: Product Management plugin

8 skills covering the core PM loop: specs, roadmap, research, stakeholders, metrics. Built for Cowork, also works in Claude Code. Connects to Slack, Linear, Jira, Notion, Figma and more.

- **Source:** [anthropics/knowledge-work-plugins/product-management](https://github.com/anthropics/knowledge-work-plugins/tree/main/product-management)
- **License:** Apache-2.0
- **Install:** `claude plugins add knowledge-work-plugins/product-management`

| Skill | What it does | Use it when |
|---|---|---|
| `write-spec` | Turns a problem statement or idea into a structured PRD: goals, non-goals, metrics, acceptance criteria | A vague ask needs to become a doc engineering can scope |
| `synthesize-research` | Clusters interviews, surveys and tickets into themes, ranked by frequency and impact | You're sitting on a pile of notes and need the "so what" |
| `stakeholder-update` | Writes one update in several versions: exec brief, engineering detail, customer-facing | Weekly status, a launch, or escalating a risk |
| `roadmap-update` | Adds, reprioritizes or re-dates roadmap items, or builds Now/Next/Later from scratch | New info lands and something has to move |
| `sprint-planning` | Sizes the backlog against real capacity (PTO, meetings), sets P0 vs stretch | Kicking off a sprint or handling carryover |
| `metrics-review` | Trend analysis against targets, turns numbers into a scorecard with actions | Weekly/monthly review, or a sudden spike or drop |
| `competitive-brief` | Competitor or feature-area brief: comparison, positioning, where to differentiate | Strategy calls, battle cards, board prep |
| `product-brainstorming` | Sparring partner for problem spaces: HMW, JTBD, first principles, opportunity trees | Before converging on a direction, to stress-test an idea |

## Matt Pocock: Skills for Real Engineers

Built for engineers, and most of the repo is engineering skills. Go straight to the **`skills/productivity`** folder: generic skills that work for almost any knowledge worker, PMs included.

- **Source:** [mattpocock/skills](https://github.com/mattpocock/skills), productivity folder: [skills/productivity](https://github.com/mattpocock/skills/tree/main/skills/productivity)
- **License:** MIT
- **Install (pick one, not both):**
  - Whole set, auto-updates: `claude plugins install mattpocock-skills`
  - Pick individual skills as editable copies: `npx skills@latest add mattpocock/skills` (the installer asks you to also take `setup-matt-pocock-skills`, do it)

**Our two picks**

| Skill | What it does | Use it when | Our take |
|---|---|---|---|
| `grill-me` | Interviews you relentlessly about a plan or design until every open decision is resolved | Before you write the spec, pitch the roadmap call, or commit to a plan you haven't stress-tested | The one that went viral. |
| `writing-for-agents` | A guide to writing docs agents read: skills, CLAUDE.md, AGENTS.md | Every time you write or edit a skill or your CLAUDE.md | A skill for writing great skills. Our favorite. |

**Getting `grill-me` to work:** the actual instructions live in a second skill, `grilling`. `grill-me` is just a `/grill-me` shortcut to it.
- Installed the plugin? You have both. Nothing to do.
- Picking skills yourself? Take `grilling`. Claude uses it whenever you say "grill me". Add `grill-me` only if you also want the `/grill-me` command.
- Only took `grill-me`? It won't work: the shortcut points to a skill that isn't there.

**Also in the folder**
- `to-questionnaire`: turns a decision you can't make alone into a questionnaire for the one person who can.
- `handoff`: compresses a long session into a doc the next session can pick up from.
- `teach`: teaches you a new concept over several sessions.
- `wait-what`: when Claude's last reply didn't land, it re-explains in plain English.

## Jesse Vincent (obra): Superpowers

A complete build method for coding agents, packaged as skills. For PMs who want to build something real and work where product meets engineering. Instead of jumping straight into code, Claude first pins down what you actually want, writes a spec, turns it into a step-by-step plan, then builds it task by task with tests and reviews along the way.

- **Source:** [obra/superpowers](https://github.com/obra/superpowers)
- **License:** MIT
- **Install (Claude Code):** `/plugin install superpowers@claude-plugins-official`
- **Use it when:** you're building a real tool or prototype and want it done the way a disciplined engineering team would, not vibe-coded.
- **The flow:** `brainstorming` (clarify the idea, agree the design) → `writing-plans` (bite-sized tasks) → build with tests → code review → finish. Each step kicks in on its own.
- **Watch out:** it's opinionated. Once installed, it steps in whenever you start building, and runs its full process every time. Great for real builds, heavy for a quick throwaway prototype.
- **Privacy note:** one optional feature loads a logo from the makers' site, which tells them only which version you use, nothing about your project. Turn it off by setting `SUPERPOWERS_DISABLE_TELEMETRY=1`.
- **Our take:** hugely popular for a reason. A great repo to check out if you want to build, not just spec.

