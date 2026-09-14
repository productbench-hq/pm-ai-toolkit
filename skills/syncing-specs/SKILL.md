---
name: syncing-specs
description: Keeps a PRD/spec in sync with decisions made elsewhere (Slack, for the workshop MVP). Classifies a new message, routes it into the right section, edits in place, and logs a dated, attributed entry. Use when asked to "sync the spec" with a new message, or run /syncing-specs.
---

# syncing-specs

## What this skill does

PMs write a spec once, then decisions happen in Slack/email/meetings and the doc goes stale. syncing-specs takes one new message and a spec doc, and brings the doc current — without silently rewriting it and without turning it into a junk drawer of unfiltered notes.

**MVP scope:** one input source (Slack, read live via MCP when triggered — not auto-listening), one hardcoded doc skeleton (below). Multiple doc types, multiple input sources, and always-on/auto-triggered syncing are explicit v2 (see `doc-sync`, not built yet).

**Trigger vs. read, kept separate on purpose:** the *trigger* stays manual — a person decides when to sync, so behavior is predictable and demoable. The *read* is live — the skill pulls the actual message(s) from the actual Slack channel, it does not work from pasted/retyped text. Manual trigger is a pacing choice; live read is what makes the sync real.

## Doc skeleton (hardcoded)

Based on Lenny Rachitsky's PRD template, plus one section his template doesn't have (Open questions — added because the skill needs somewhere to put something raised-but-unresolved without pretending it's decided; see "What NOT to do").

Every spec this skill operates on must have these nine sections, in this order. Sections may be empty — leave empty sections alone unless the new message actually fills them; don't invent content to complete a section.

1. Description — what is it?
2. Problem — what problem is this solving?
3. Why — how do we know this is real and worth solving?
4. Success — how do we know if we've solved it?
5. Audience — who are we building for?
6. What — roughly, what does this look like in the product?
7. How — what's the experiment plan?
8. When — ship date and milestones?
9. Open questions — *(not in the original template; added by this skill)*

If the doc is missing one of these nine sections, stop and ask before proceeding — don't guess a structure.

## Step-by-step process

### Step 1 — Pull the message(s)

When triggered ("sync the spec"), read the doc's frontmatter for `slack_channels: [...]` — the list of channels relevant to this initiative, set once when the doc was created. If the field is missing, stop and ask which channel(s) to use rather than guessing; don't search across the workspace.

For each channel in the list, read live via the `Slack:slack_read_channel` tool (use the fully qualified name — an unqualified `slack_read_channel` may fail to resolve if more than one Slack-like MCP server is connected):
- If the doc has a `## Sync log` with prior entries, pull messages newer than the last logged sync
- If this is the first sync, pull messages since a timestamp/message link the user points you to (don't guess a window — ask if unclear)
- If multiple new messages exist (within one channel or across channels), process them one at a time, in order — a later message may contradict an earlier one in the same batch, and Step 4 needs to catch that

Never fabricate or paraphrase-from-memory what a message said — always pull the actual text via the tool.

**Note:** this only scopes *which channels* to read for *this one doc*. It does not handle a user running multiple initiatives across overlapping channels and needing a message routed to the correct *document* — that's an explicit v2 problem, not handled here.

### Step 2 — Classify the message

Read the new message and classify it as exactly one of:

- **Decision** — something was decided/settled (an owner assigned, a scope call made, a tradeoff resolved)
- **New requirement** — a new thing the product must do, or a change to an existing one
- **Open question** — something raised but not resolved
- **Noise** — social chatter, logistics, anything with no bearing on the spec

Classify before routing. Don't skip straight to editing — a wrong classification here is the most common failure mode (e.g. treating a tentative idea as a settled decision).

### Step 3 — Handle noise

If classified as noise: discard it. Do not log it, do not mention it in the diff, do not add it to Open questions "just in case." A noise entry showing up anywhere defeats the point — the doc must stay a signal, not an inbox.

### Step 4 — Check for contradiction

Before routing, check whether the message conflicts with something already written in the doc (e.g. Audience assumed all users, and the new message carves out an exception). If it does:

- Don't just append the new content next to the old
- Edit the existing line so the doc reflects the current, reconciled state
- Call out the resolution explicitly in the diff and log ("narrowed from all users to X, per [message]") — this is the moment that should be visible, not buried

### Step 5 — Route and edit

Route the message to exactly one section. Rough mapping by content, not by classification alone — read what the message is actually about:

- **What** — product/flow shape ("build X", "the screen should do Y")
- **How** — experiment/test design (control, treatment, audience split for the *test itself*, rollout mechanics)
- **Audience** — who's in scope (a carve-out or expansion of which users this applies to)
- **Success** — what counts as solved / the metric bar
- **When** — ship date, milestones
- **Why** — new evidence that changes confidence the problem is real/worth solving
- **Description, Problem** — rarely edited by a single message; flag instead of auto-editing unless the message unambiguously redefines what the project *is* or the problem itself, not just a detail of it

Open question → adds a bullet to Open questions, phrased as a question — this is the one section that isn't part of Lenny's original template, reserved for exactly this.

Decision → usually resolves something already sitting in Open Questions, or overwrites/narrows an existing line in whichever section it actually changed (see Step 4).

Never do a full-document regeneration. Touch only the section(s) the message actually affects — including cross-references. If an edit makes a mention or pointer *elsewhere* in the doc (e.g. "see Open questions" in Why) stale or wrong, don't fix it yourself — that section wasn't what the message was about. Instead add a bullet to Open questions flagging the stale reference, so a human resolves it deliberately.

### Step 6 — Show the diff

Present a before/after diff of the section(s) touched — not the whole doc. Make added/changed/removed content visually distinguishable.

### Step 7 — Log the change

Append one line to a `## Sync log` section at the bottom of the doc (create it if it doesn't exist yet — it is never removed or rewritten by this skill, only appended to):

`- [date] — [section]: [one-line summary of what changed] — source: "[message excerpt]"`

This is what makes the sync trustworthy: every edit to the narrative is traceable, permanently, even after the demo/session ends. Never overwrite or edit a past log entry.

## Examples

**Example 1 — Decision with contradiction**

Input: "given the D0 auto-route test tanked retention when content didn't match expectations, let's not force everyone straight into lesson 1 — keep the plan view for anyone who skipped goal-setting in onboarding"

Classification: Decision (with contradiction — narrows Audience)

Action: Edit the Audience line that said "applies to all new users" to carve out the goal-setting exception. Log the change with the source quote.

**Example 2 — Clean new requirement, single section**

Input: "users should land in a quick assessment first — goal is to use it to adjust learn path question difficulty"

Classification: Decision/new requirement, no contradiction — but touches two sections at once (adds to What, and separately resolves an existing Open Questions bullet it answers). Route and log each section touched separately; don't force it into one.

**Example 3 — Noise**

Input: "lol did anyone see the new coffee machine in the office"

Classification: Noise

Action: Discard. No edit, no diff, no log entry. Nothing in the doc changes.

## What NOT to do

- Don't force content into an empty section to "complete" it
- Don't log noise
- Don't rewrite sections the message doesn't touch
- Don't silently overwrite — always show the diff and log entry
- Don't invent a doc structure if the nine sections aren't present — ask instead
