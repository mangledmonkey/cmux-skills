# cmux-skills

Claude Code plugin that provides [cmux](https://github.com/manaflow-ai/cmux) skills for topology control, browser automation, markdown viewing, and debug windows.

## Install

```bash
claude plugin add mangledmonkey/cmux-skills
```

## Skills

| Skill | Description |
|-------|-------------|
| **cmux** | Control windows, workspaces, panes, and surfaces — create, focus, move, reorder, identify |
| **browser** | Automate cmux webview surfaces — open sites, snapshot interactive refs, fill forms, click, wait |
| **markdown** | Open markdown files in a formatted panel with live file watching alongside the terminal |
| **debug-windows** | Manage Sidebar/Background/Menu Bar Extra debug windows and capture combined snapshots |

## How It Works

Each skill is a `SKILL.md` with YAML frontmatter that Claude Code loads on demand. Skills include reference docs for deep-dive topics and shell templates/scripts for common workflows.

Skills are sourced from the upstream [manaflow-ai/cmux](https://github.com/manaflow-ai/cmux) repository. A GitHub Actions workflow (`.github/workflows/sync-upstream.yml`) syncs them weekly and opens a PR with any changes.

## License

See the upstream [cmux](https://github.com/manaflow-ai/cmux) repository for license information.
