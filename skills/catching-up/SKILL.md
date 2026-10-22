---
name: catching-up
description: Scans your connected tools (email, calendar, tracker, docs, meeting notes) for what changed since the last catch-up, reports what needs you and which records are now out of date, and proposes updates for a yes. Use at the start of a work session, when asked "what did I miss" or "catch me up", or run /catching-up.
disable-model-invocation: true
---

# catching-up

**What it does:** Notices what changed in your work before you have to point it out: the stakeholder email that moved a deadline, the meeting that got cancelled, the ticket someone closed. It reads your connected tools, tells you what matters, and offers to fix any record that's now wrong. Scan → report → confirm → write.
**Not in scope:** choosing what to work on, replying to anything, editing events or tickets you don't own, connecting new tools.

**Rule: never write to a file or a tool before the user approves that specific update, by letter.**

Your sources, the records to keep true, and where decisions go live in [setup.md](setup.md), next to this file. Step 1 fills it on first run.

## State

`.claude/state/last-catch-up` holds the time of the last scan, ISO UTC. Keep it out of version control. Optional: [nudge.sh](nudge.sh) is a SessionStart hook that asks for a catch-up when the last one is over 4 hours old; `setup.md` says how to switch it on.

## Steps

1. **Check setup.** Read `setup.md`. If nothing is connected to Claude, stop and say so. If any field is empty:
   a. Look up what the connected tools can tell you: which email, calendar, tracker and docs tools exist, and the tool names for each job.
   b. Ask the user, in one message, for the rest: who counts as a stakeholder, which records to keep true, and where decisions go.
   c. Show the filled `setup.md`. Write it on a yes.
   Done when: every field in `setup.md` is filled or marked "none".

2. **Window.** Start = the timestamp in `.claude/state/last-catch-up`, or 3 days ago if the file is missing. End = now.
   Done when: you have both.

3. **Gather.** One read-only pass per source in setup:

   | Source | What to keep |
   |---|---|
   | Email, Slack, Teams | Messages from people in the window: stakeholders, your team, users, threads you're cc'd on. Keep notices that ask for a decision. Drop newsletters, promotions and tool notifications |
   | Calendar | Events from now to +14 days created or updated in the window: new bookings, cancellations, declines, moved times. Plus every upcoming event you haven't answered, however old the invite |
   | Tracker | Issues changed in the window that are assigned to you, created for you by others, or changed status |
   | Docs and files | Docs modified in the window by someone other than you |
   | Meeting notes | New notes or transcripts in the window |

   Done when: every source was read, or a failing one is named for the report.

4. **Match.** Tie every kept signal to a record from setup: a priority, a project, an issue in your tracker. A signal that matters but has no record is **new**.
   Done when: every kept signal has a record or is marked new.

5. **Classify.** Each matched signal is one of three, or it's dropped:
   - **Needs you:** a reply, decision or review is waiting on you.
   - **Changed:** a date moved, a meeting was booked or cancelled, someone created, moved or closed something.
   - **Out of date:** a record now disagrees with its source, e.g. the email says the launch slipped and your priorities file still has the old date.

6. **Report.** Plain chat, most important first, every line linking to its source (message, event, issue or file):

   ```
   Catch-up since <day hh:mm>

   Needs you
   1. ...

   Changed
   - ...

   Proposed updates
   A. <record>: <exact change>
   ```

   Propose an update only when the record would otherwise be wrong or steer later work: a priority, a date, a decision (to the decisions destination in setup), a new commitment (a tracker issue; check it doesn't already exist). Logistics you already know stay in the report and go no further. Nothing kept → one line: `Nothing new since <day hh:mm>.`

7. **Stamp.** Right after the report: `mkdir -p .claude/state && date -u +%Y-%m-%dT%H:%M:%SZ > .claude/state/last-catch-up`. The scan happened, whatever gets approved.

8. **Confirm and apply.** The user answers by letter ("apply A, C"). Apply exactly those, then report one line per change. If the user opened the session with a request, carry on with it.
   Done when: every approved letter is applied, or reported as failed.

## Quality bar

- [ ] Every source in setup read, or the failing one named
- [ ] Every report line links to its source
- [ ] Every write traced to a letter the user approved
- [ ] Short enough to read in 30 seconds when nothing major happened

## Example

Lena, Senior PM at Hivelist. Sources: Gmail, Google Calendar, Linear. Records: `priorities.md`. Decisions go to `knowledge/log.md`.

```
Catch-up since Mon 09:12

Needs you
1. Nordhafen's account lead asks whether export can ship by 15 Nov, not end of November. Reply needed (email link)
2. Design review Thu 14:00, invite not answered (event link)

Changed
- Jonas closed HIV-57 "Confirm November export deadline" (issue link)
- Weekly sync moved from Tue to Wed 10:00 (event link)

Proposed updates
A. priorities.md: export deadline "end of November" → "15 Nov, if confirmed"
B. knowledge/log.md: decision: notifications move to Q1 (from the sync notes)
```

**User:** "Apply B. A waits until I've replied."
