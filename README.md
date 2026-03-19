# cmux-skills

Give Claude Code full control of your [cmux](https://github.com/manaflow-ai/cmux) workspace — split panes, automate browsers, display docs, all from natural language.

[cmux](https://github.com/manaflow-ai/cmux) is a macOS terminal with split panes, embedded browsers, and a markdown viewer built in — designed for AI agents to control programmatically.

Claude Code doesn't know any of that. It just sees a shell. This plugin teaches it what cmux can do.

## Install

```
/plugin marketplace add mangledmonkey/cmux-skills
/plugin install cmux
```

## What Claude gains

- Split panes and arrange your workspace by asking
- Open a browser beside your terminal and debug web apps visually
- Display docs, plans, or notes in a live-updating markdown panel
- Capture debug window snapshots with a single command

## Skills

### cmux

Control windows, workspaces, panes, and surfaces — create, focus, move, reorder, identify.

> **You say:** "Split my workspace and move the browser to the right pane"
>
> **Claude runs:**
> ```bash
> cmux new-split right --panel pane:1
> cmux move-surface --surface surface:7 --pane pane:2 --focus true
> ```

### browser

Automate cmux webview surfaces — open sites, snapshot interactive refs, fill forms, click, wait.

> **You say:** "Open my dev server and check if the login form works"
>
> **Claude runs:**
> ```bash
> cmux browser open http://localhost:3000 --json
> cmux browser surface:7 snapshot --interactive
> cmux browser surface:7 fill e1 "test@example.com"
> cmux browser surface:7 click e2 --snapshot-after
> ```

### markdown

Open markdown files in a formatted panel with live file watching alongside the terminal.

> **You say:** "Show my plan file in a side panel"
>
> **Claude runs:**
> ```bash
> cmux markdown open plan.md
> # Panel auto-updates when the file changes on disk
> ```

### debug-windows

Manage Sidebar, Background, and Menu Bar Extra debug windows. Capture combined snapshots.

> **You say:** "Grab a snapshot of all debug windows"
>
> **Claude runs:**
> ```bash
> ${CLAUDE_PLUGIN_ROOT}/skills/debug-windows/scripts/debug_windows_snapshot.sh --copy
> ```

## Cross-skill orchestration

The real power is combining skills. Ask Claude to write a reusable script and it will wire them together.

> **You say:** "Write a script that sets up a research workspace — takes a URL, opens it in a browser pane, and puts a notes file beside it"

**Claude writes:**

```bash
#!/usr/bin/env bash
set -euo pipefail

URL="${1:?usage: research.sh <url> [notes-file]}"
NOTES="${2:-research-notes.md}"

# Start the notes file if it doesn't exist
[ -f "$NOTES" ] || printf "# Research Notes\n\n## Source\n%s\n" "$URL" > "$NOTES"

# Open browser to the target URL
RESULT=$(cmux --json browser open "$URL")
SURFACE=$(echo "$RESULT" | jq -r '.surface_id')
cmux browser "$SURFACE" wait --load-state complete --timeout-ms 15000

# Open notes panel beside it
cmux markdown open "$NOTES"

# Flash the browser to confirm
cmux trigger-flash --surface "$SURFACE"
```

Three skills, fifteen lines, fully parameterized.

## Requirements

- [cmux](https://github.com/manaflow-ai/cmux) (macOS 14.0+)
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code)

## Upstream sync

Skills are sourced from [manaflow-ai/cmux](https://github.com/manaflow-ai/cmux) and synced weekly via GitHub Actions. A Claude CLI step reviews each sync PR before merge to fix paths and references.

## License

See the upstream [cmux](https://github.com/manaflow-ai/cmux) repository for license information.
