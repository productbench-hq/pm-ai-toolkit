# catching-up setup

Filled on first run by `/catching-up`. Edit any time. Leave a field empty and the skill asks again.

## Sources

One row per connected tool. Remove rows you don't use.

| Source | Tool | Tool names for each job |
|---|---|---|
| Email | <!-- Gmail, Outlook --> | <!-- search, read thread --> |
| Chat | <!-- Slack, Teams, or "none" --> | |
| Calendar | <!-- Google Calendar, Outlook --> | <!-- list events --> |
| Tracker | <!-- Linear, Jira, Asana --> | <!-- list issues --> |
| Docs | <!-- Google Drive, Notion, or "none" --> | <!-- list recently modified --> |
| Meeting notes | <!-- Granola, Fireflies, or "none" --> | |

- **Timezone:** <!-- e.g. Europe/Berlin -->
- **Stakeholders:** <!-- names or domains whose messages always count, e.g. your manager, key customers -->

## Records to keep true

| Record | Where | What it holds |
|---|---|---|
| Priorities | <!-- e.g. priorities.md --> | <!-- this quarter's goals and dates --> |
| Decisions | <!-- e.g. knowledge/log.md, a doc link --> | <!-- how to add: e.g. append "- decision: <text> — <source>, <date>" --> |

## Optional: the nudge hook

To get asked for a catch-up automatically when the last one is over 4 hours old, add this to `.claude/settings.json` in your project, adjusting the path to where you put this skill:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "startup|resume",
        "hooks": [
          { "type": "command", "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/skills/catching-up/nudge.sh" }
        ]
      }
    ]
  }
}
```

Then run `chmod +x .claude/skills/catching-up/nudge.sh`, and add `.claude/state/` to `.gitignore`.
