# resume setup

Filled on first run by `/resume`. Edit any time. Leave a field empty and the skill asks again.

## Projects

- **Where project files live:** <!-- e.g. projects/<name>/README.md, or a Notion database -->
- **Status fields:** <!-- what marks a project active, paused or closed, e.g. frontmatter `status:` and `next_milestone:`, or a "Status:" line -->
- **Recent changes:** <!-- e.g. git log on the project folder, or "none" -->
- **Decision log:** <!-- e.g. knowledge/log.md, a doc link, or "none" -->

## Sources

One row per connected tool. Remove rows you don't use.

| Source | Tool | Tool names for each job |
|---|---|---|
| Tracker | <!-- Linear, Jira, Asana, or "none" --> | <!-- list issues --> |
| Calendar | <!-- Google Calendar, Outlook, or "none" --> | <!-- list events --> |
| Email | <!-- Gmail, Outlook, or "none" --> | <!-- search, read thread --> |

- **Timezone:** <!-- e.g. Europe/Berlin -->

## Tip

`resume` is fastest when each project file opens with a few status fields, for example:

```yaml
---
status: active                       # active | paused | closed
next_milestone: 2026-11-15 Export live for Nordhafen
close_when: "Nordhafen exporting weekly without support"
---
```
