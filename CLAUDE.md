# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

This is a **Claude Code plugin** that packages skills for [cmux](https://github.com/manaflow-ai/cmux), a macOS multi-pane terminal/webview topology manager. The plugin is defined in `.claude-plugin/plugin.json` and exposes four skills to Claude Code sessions.

## Architecture

```
.claude-plugin/plugin.json   ← Plugin manifest (name, version, description)
skills/
  cmux/                      ← Topology control (windows, workspaces, panes, surfaces)
  browser/                   ← Browser automation on cmux webview surfaces
  debug-windows/             ← Debug window management and snapshot capture
  markdown/                  ← Markdown viewer panel with live file watching
```

Each skill directory follows the same structure:
- `SKILL.md` — Frontmatter (`name`, `description`) + skill content loaded by Claude Code
- `references/` — Deep-dive docs loaded on demand from SKILL.md links
- `agents/` — `openai.yaml` defining the skill's OpenAI-compatible agent interface
- `templates/` or `scripts/` — Executable shell scripts (browser has templates/, debug-windows has scripts/)

## Upstream Sync

Skills are sourced from `manaflow-ai/cmux` and synced via `.github/workflows/sync-upstream.yml`:
- Runs weekly (Monday 9am UTC) or on manual dispatch
- Upstream skill directories use `cmux-` prefixes; this repo strips them (e.g. `cmux-browser` → `browser`)
- After copying, a Claude CLI step reviews and fixes: `${CLAUDE_PLUGIN_ROOT}` path references, frontmatter `name` fields matching local directory names, and cross-skill relative links
- All `.sh` files are made executable after sync

## Key Conventions

- **Skill names** in SKILL.md frontmatter must match the directory name (e.g. `name: browser`, not `name: cmux-browser`)
- **Script paths** in skill content must use `${CLAUDE_PLUGIN_ROOT}/skills/...` — not bare relative paths — so they resolve correctly when the plugin is installed
- **Handle refs** use short format by default: `window:N`, `workspace:N`, `pane:N`, `surface:N`
- **Cross-skill links** use relative markdown paths (e.g. `../browser/SKILL.md`)

## Working With Skills

When editing SKILL.md files, preserve the YAML frontmatter (`name` and `description` fields). The `description` field is what triggers skill activation in Claude Code — it should clearly state when the skill applies.

When adding reference docs, link them from the SKILL.md "Deep-Dive References" table so they're discoverable but loaded on demand (not inlined into the skill).
