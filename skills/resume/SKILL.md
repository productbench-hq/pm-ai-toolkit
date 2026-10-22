---
name: resume
description: Gets you back into one project in a briefing of at most 6 lines (status, where it stands, last decision, waiting on, next, coming up), read from the project's file, its recent changes, your decision log, tracker, calendar and email. Read-only. Use when switching back to a project, asked "where was I on <project>?", or run /resume <project>.
disable-model-invocation: true
---

# resume

**What it does:** Picks up a project where you left off, so you don't have to re-explain it or reread everything. One parallel read, one short briefing.
**Not in scope:** choosing what to work on, updating tasks, replying to anyone, summarising a project's whole history.

**Rule: read-only. The one exception is the `Out of date` line: fix that record only after the user says yes.**

Where your projects live and which tools to read are in [setup.md](setup.md), next to this file. Step 1 fills it on first run.

## Steps

1. **Check setup.** Read `setup.md`. If any field is empty:
   a. Look up what the tools can tell you: which tracker, calendar and email tools are connected, and the tool names for each job.
   b. Ask the user, in one message, for the rest: where project files live, and where decisions are logged.
   c. Show the filled `setup.md`. Write it on a yes.
   Done when: every field is filled or marked "none".

2. **Pick the project.** Match the argument loosely against the projects in setup (`export` → `export-nordhafen`). No argument or no match → reply `Which project?` with the project names, and stop.
   Done when: exactly one project is picked.

3. **Gather, in one parallel pass.** Skip any source marked "none" in setup.

   | Source | What to read |
   |---|---|
   | Project file | Status fields and the opening paragraphs |
   | Recent changes | The project's last 5 changes, e.g. `git log -5 --format='%cs %s' -- <project folder>` |
   | Decision log | The newest decision that names the project |
   | Tracker | The project's open issues: title, owner, due date |
   | Calendar | Events in the next 14 days that mention the project or its contacts |
   | Email | Threads from the last 14 days, searched by project name **and** contact names (clients often write from another domain). Open a thread only when its snippet suggests someone is waiting on someone |

   Paused or closed project → skip calendar and email.
   Done when: every source was read, or a failing one is named on the briefing's last line.

4. **Reply with the briefing.** Plain chat, every line linked to its source:

   ```
   **<Project>** · <status> · last touched <day>
   Where it stands: <one line>
   Last decision: <one line> (<date>)
   Waiting on: <who, what, since when>
   Next: <date> <issue> · <date> <issue>
   Coming up: <day time> <meeting>
   Out of date: <field> says X, <source> says Y. Fix it?
   ```

   - Drop a line with nothing to say. Never write "none".
   - Last touched = the newest of the project's last change and its latest tracker update.
   - `Out of date` only when the project file disagrees with a source (a milestone that's passed, a status the emails contradict).
   - Nothing after the briefing. No recap, no suggestions.

## Example

Lena, Senior PM at Hivelist. Projects in `projects/<name>/README.md`, tracker Linear, Google Calendar, Gmail.

**User:** `/resume export`

```
**Export for Nordhafen** · active · last touched Tue 13 Oct
Where it stands: PRD v1 shared, design mockups in review; target end of November
Last decision: CSV first, Excel later (8 Oct)
Waiting on: Nordhafen's account lead, sample export files (Lena asked 9 Oct)
Next: 16 Oct HIV-51 mockup review · 20 Oct HIV-55 PRD v2
Coming up: Thu 15 Oct 14:00 Design review
Out of date: README says target "end of November", Nordhafen's email of 12 Oct asks for 15 Nov. Fix it?
```

**User:** "Not yet, I'll reply first."
