---
name: plan-my-day
description: Plans the rest of today, or tomorrow, as time blocks around existing meetings. Looks back at how the last plan went, pulls calendar events and open tasks (from a connected tracker like Linear, Jira or Asana, or asked in chat), drafts outcome-based blocks, and writes them to the calendar only after a yes. Works with Google Calendar or Outlook. Use when asked "what's next", "plan my day" or "plan tomorrow", or run /plan-my-day.
disable-model-invocation: true
---

# plan-my-day

**What it does:** Turns your calendar gaps and open tasks into a short plan of time blocks, each with a finish line. Draft → confirm → write.
**Not in scope:** moving or editing existing meetings, planning a whole week, creating or re-prioritizing tasks in the tracker, running on a schedule.

**Rule: never create or change a calendar event before the user says yes to this specific plan.**

Your calendar, timezone, working hours and tracker live in [setup.md](setup.md), next to this file. Step 1 fills it on first run.

## Steps

1. **Check setup.** Read `setup.md`. If no calendar is connected to Claude, stop and say so. If any field is empty:
   a. Look up what the tools can tell you: calendars, timezone, tracker (if one is connected), its statuses. Note the tool names used for each job.
   b. Ask the user, in one message, for the rest: working hours, which tracker statuses mean "committed" (e.g. Todo, In Progress), and whether to use a tracker at all.
   c. Show the filled `setup.md`. Write it on a yes.
   Done when: every field in `setup.md` is filled or marked "none".

2. **Pick the window.** Default: the rest of today, from now. Plan tomorrow instead if today's working hours are over, or the user says "tomorrow". If it's close to the end of the day and unclear, ask.
   Done when: the window is a date plus start and end time.

3. **Look back.** Find the most recent day before the window with blocks carrying the marker (see Reference), within the last 7 days. None → skip this step silently.
   For each marked block without an `outcome:` line:
   - Linked task is now done in the tracker → **hit**.
   - Otherwise → ask. One numbered list (block, its "done when"), answered by number in one reply.
   The user's answers are the approval to record them: add `outcome: hit` or `outcome: missed` to each block's description.
   Done when: every marked block has an outcome, and one line is ready: `Last plan (<day>): <n> hit, <n> missed`. Missed tasks carry forward into step 5.

4. **Pull the window.**
   - **Calendar:** every event in the window. These are fixed. Never move or change them.
   - **Tasks, with a tracker:** open tasks assigned to the user in a committed status, plus any overdue or due in the window. Fields: title, priority, due date, status, and a "done when" line if the description has one.
   - **Tasks, without a tracker:** ask "What are the 3 things that must move today?"
   Done when: you have the fixed events and a task list.

5. **Draft the plan.**
   - Fixed events are anchors. List them, don't touch them.
   - Order the work: carry-overs from step 3, then overdue or due in the window, then by priority.
   - **Every block names its outcome:** "done when v1 of the export PRD is shared with Jonas," not "work on PRD." If you can't name one, ask.
   - **Admin slot:** small tasks (under ~30 min: replies, quick fixes) share one block, listed inside it. Only big work gets its own block.
   - **Don't cram.** Leave breathing room. If something overdue or urgent doesn't fit, say so. Don't drop it silently.
   - One line per block: time range, what, done when, task link.
   Show it as plain chat text: the look-back line first, then the plan.
   Done when: every free slot you fill has a block with an outcome, and anything that didn't fit is named.

6. **Confirm.** Ask: "Look right, or anything to move, drop or add?"
   Done when: the user gives a clear yes to this draft. If they give edits, apply them and show the plan again.

7. **Write to the calendar.** One event per new block. Skip anything that was already an event.
   - Title: short task name (the tracker title if it came from there, `Admin` for the admin slot)
   - Time: from the confirmed plan, in the timezone from setup
   - Description: the block marker
   - Shown as busy. No attendees, no video link.
   If an event fails, stop and say which one. Don't retry silently.
   Done when: every block is created, or reported as failed.

8. **Report.** One line per block created: `09:00–10:30 · Draft export PRD · done when v1 shared with Jonas (HIV-55)`. No extra prose.

## Reference

**Block marker.** Every block this skill writes has this in its description. It's how step 3 finds past blocks.
```
planned by plan-my-day
done when: <outcome>
<task link, if any>
```
Step 3 later adds one line: `outcome: hit` or `outcome: missed`.

**Before writing to the calendar, check**
- Every existing event is untouched. The plan only fills gaps.
- The yes was for this draft, not an earlier one.
- Every block has a concrete "done when."
- Nothing overdue or urgent was dropped without saying so.
- The plan is a list to glance at, not a report.

## Example

Lena, PM at Hivelist. Tuesday 2026-09-15, 08:45. Google Calendar + Linear.

**Step 3 (look back):**
> Monday's plan had 3 blocks. HIV-42 is done in Linear, so that's a hit. Two to check:
> 1. Admin, done when: 5 customer replies sent
> 2. Export mockups review, done when: comments sent to design
>
> Hit or missed?

**User:** "1 hit, 2 missed"

**Step 5 (plan):**
> Last plan (Mon): 2 hit, 1 missed
>
> - 09:00–10:00 · **Standup + roadmap sync** (fixed)
> - 10:00–11:00 · Export mockups review · done when comments sent to design (HIV-51, carried over)
> - 11:00–12:30 · Draft export PRD · done when v1 shared with Jonas (HIV-55, due Wed)
> - 12:30–13:30 · **Lunch** (fixed)
> - 13:30–14:00 · **Nordhafen call** (fixed)
> - 14:00–14:30 · Admin · done when: Nordhafen follow-up sent, 2 bug triages, Q4 survey reply
> - 14:30–16:00 · Notifications interview synthesis · done when top 3 themes posted in #product (HIV-48)
>
> Didn't fit: HIV-60 "Update pricing page copy" (due Friday). Tomorrow?
>
> Look right, or anything to move, drop or add?

**User:** "Yes."

**Step 8 (report):**
> - 10:00–11:00 · Export mockups review · done when comments sent to design (HIV-51)
> - 11:00–12:30 · Draft export PRD · done when v1 shared with Jonas (HIV-55)
> - 14:00–14:30 · Admin · done when: 3 items done
> - 14:30–16:00 · Notifications interview synthesis · done when top 3 themes posted (HIV-48)
