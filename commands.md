# Commands worth knowing

Claude Code has built-in slash commands for controlling your session. Type them in the chat box. These five work in both the desktop app and the terminal.

| Command | What it does | Use it when |
|---|---|---|
| `/context` | Shows what's filling your context window, by category: instructions, tools, connectors, memory files, skills, conversation | You want to see where your tokens go, e.g. too many connectors eating space before you've typed a word |
| `/compact` | Condenses the conversation so far into a summary that keeps the key info, so Claude doesn't lose the thread and you get space back. Add a focus: `/compact keep the decisions and open questions` | Your context is about 60% full (see below) |
| `/clear` | Starts a fresh conversation. Your CLAUDE.md and memory files still load. The old conversation stays findable via `/resume` | You finish one task and start the next |
| `/rewind` | Takes you back to an earlier point: the conversation, your files, or both. Shortcut: press `Esc` twice | Claude went down the wrong path and you want to try again from before it did |
| `/resume` | Picks up an earlier conversation from a list. In the terminal, `claude --continue` reopens the last one | You want to continue yesterday's work |

## Our rules of thumb
- **Compact at 60%.** Don't wait for Claude to compact on its own when the window is nearly full. Compact early, on your terms, and tell it what to keep.
- **Clear between tasks.** One task, one conversation. Leftovers from the last task just add noise to the next one.
- **Rewind has limits.** It can't undo anything that left your machine (a sent Slack message, a created Linear ticket), and it doesn't track file changes Claude made by running terminal commands.

**Bonus:** `/btw <question>` asks a quick side question without adding it to the main conversation.

## Advanced: terminal-only commands

Some commands open settings panels that only run in the terminal version of Claude Code (type `claude` in your Terminal). In the desktop app, use the app's own settings instead.
- `/config`: your settings
- `/permissions`: what Claude is allowed to do without asking you first
- `/hooks`: actions that run automatically, e.g. a check every time Claude edits a file
- `/mcp`: manage your connectors
- `/doctor`: checks your install when something's off
