---
name: experiment-brief
description: Drafts an experiment brief (hypothesis, metrics, A/B design, sample size) from a research finding, a problem or an idea. Checks every claim against the original source, hunts for counter-cases, looks for past and overlapping tests, and marks every unknown with an owner. Use when asked for an experiment brief or a test design, or to turn a problem into an experiment. Prefer it over generic spec writing for anything that will be A/B tested. Or run /experiment-brief.
disable-model-invocation: true
---

# experiment-brief

**What it does:** Turns one problem into one brief that is ready for review: verified evidence, past test results, a falsifiable hypothesis and a complete design table.
**Not in scope:** setting up the test in your experimentation tool, analyzing results, prioritizing between briefs (order and approval are the user's call), PRDs for things that won't be tested.

**One brief tests one change:** the smallest thing that could move the metric on its own. Parts that do nothing without each other (asking a question and routing on the answer) are one change. Given several changes, write one brief each.

Your team, North Star, research, analytics tool and file locations live in [setup.md](setup.md). The brief layout lives in [template.md](template.md).

## Steps

1. **Check setup.** Read `setup.md`. If any field is empty, ask the user for the missing ones in one message, show the filled file, and write it on a yes.
   Done when: every field in `setup.md` is filled or marked "none".

2. **Scope.** Write the problem in one sentence and the change in one sentence. They become the brief's TL;DR.
   Check the pipeline (from setup) for an existing row on this change. If there is one, this is a **redraft**, handled in step 8.
   Done when: both sentences exist. If the input is too vague to name a change, ask before continuing.

3. **Evidence.** Find research that matches the problem in the research location from setup. **Verify every claim against the original source** (the transcript, ticket or survey response), not a summary of it. If the source doesn't support what the summary says, keep the row, mark it **weak**, and say so in the reply.
   Hunt for three kinds of **counter-case**:
   - the problem is absent for this user
   - the current product already works for them
   - the change could hurt them
   Done when: every evidence row cites a source you opened, and each counter-case kind has a row or "none found". If only one user directly shows the problem, say so in the TL;DR.

4. **History.** Search the past-experiments location from setup, the pipeline and other briefs for tests that touch the same surface, mechanic or metric.
   Done when: each related test is named with its result (or status, if not run yet) and how this design responds, or "no related tests found" is stated. Any test with the same audience in an overlapping window is a **conflict**: list it under open questions with "run in sequence, or make the audiences exclusive?"

5. **Draft.** Fill `template.md` from top to bottom.
   - Hypothesis: *If we [change], [metric] will [move], because [evidence].*
   - Primary metric: the North Star from setup, if this change can plausibly move it. Otherwise use the metric it does move, and state that it doesn't serve the North Star.
   - At least one guardrail: the metric this change could hurt.
   - Every number traces to something you can cite: a source, the analytics tool, a past test. Numbers without an origin are dropped. Every unknown is `TBD` plus what unblocks it.
   Done when: no cell in the metrics or design tables is empty, and every `TBD` (frontmatter included) has an owner in the open-questions table.

6. **Size.** With the analytics tool from setup connected: pull the primary metric's baseline for the audience and the weekly number of eligible users, then compute sample size and runtime with the formula in `template.md`.
   Without it: MDE and runtime stay `TBD`, blocked on the baseline, owner from setup. The brief can't move to `Running` until they're filled.
   Done when: MDE and runtime hold a computed value, or a `TBD` with its blocker.

7. **Show and wait.** Reply with: the TL;DR, hypothesis, primary metric and guardrail, related tests and conflicts, weak evidence, and open questions grouped by owner. Ask: "Save the brief and add it to the pipeline?"
   Done when: the user says yes. If they give edits, apply them and show the changed parts again.

8. **Save.**
   a. Save the brief to the briefs folder from setup as `<slug>.md`.
   b. Update the pipeline (skip if setup says "none"):
      - **New brief:** add a row with status `Draft` and order `—`.
      - **Redraft:** relink the existing row and keep its order. If the evidence, hypothesis or design changed materially, set status back to `Draft` and tell the user it needs approval again.
      - Change order or approval only on the user's word.
   c. If an earlier version of this brief lives elsewhere, add one line under its heading: `> Superseded by <path> on <date>.`
   d. Set the frontmatter `status` to match the pipeline.
   Done when: the brief, the pipeline row and any superseded pointer exist. Report the paths in one line each.

## Example

Hivelist, a task app for teams. North Star: weekly active teams. Analytics: not connected.

**Input:** "Interviews keep saying new admins don't invite their team. Turn that into an experiment."

**Step 7 reply:**
> **TL;DR:** Prompting new admins to invite 3 teammates right after creating their first project should raise week-1 team activation. Evidence is solid (4 of 9 interviews), with one counter-case.
>
> **Hypothesis:** If we show an invite prompt right after the first project is created, the share of new workspaces with 3+ active members in week 1 will rise, because 4 of 9 admins said they planned to invite the team "once it was set up" and never came back to it.
>
> **Primary:** new workspaces with 3+ active members in week 1 (serves the North Star). **Guardrail:** first-project completion rate.
>
> **Related tests:** "Onboarding checklist" (shipped in Q2, +3% project creation, no effect on invites). This design moves the ask to the moment of highest intent, not a checklist item. **Conflict:** "Template gallery" runs on new workspaces through October. Run in sequence, or make the audiences exclusive?
>
> **Weak evidence:** the summary says interview 6 "asked for bulk invite", but the transcript only mentions Slack sync.
>
> **Open questions**
> - Lena: baseline for 3+ active members, weekly new workspaces (blocks MDE and runtime)
> - Jonas: sequence with Template gallery, or exclusive audiences?
>
> Save the brief and add it to the pipeline?
