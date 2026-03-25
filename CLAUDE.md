# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **mise configuration repository** that centrally manages AI CLI tools and development language/runtime versions via [mise](https://mise.jdx.dev/). It is not a traditional software project — there is no build system, no tests, and no application code. The primary artifact is `mise.toml`.

## Key Files

- `mise.toml` — Main configuration declaring all managed tools and their versions
- `README.md` — Bilingual landing page linking to both language doc sets
- `doc/zh/` — Chinese documentation (README, tutorial, per-language guides)
- `doc/en/` — English documentation (README, tutorial, per-language guides)

## Common Commands

```bash
# Install all tools declared in mise.toml
mise install

# List installed tools and their versions
mise list

# Upgrade a specific tool to latest
mise use <tool>@latest

# Check which tools are outdated
mise outdated

# See where mise resolves a tool from
mise where <tool>
```

## How This Repo Is Used

The `mise.toml` here is symlinked to the global mise config (`~/.config/mise/config.toml`), making all declared tools available system-wide. Changes to `mise.toml` affect the user's global development environment.

## Tool Backends

Mise uses different backends to install tools:

| Backend | Tools | Notes |
|---------|-------|-------|
| `core` | python, java, node, rust | Built-in to mise |
| `aqua` | claude, codex, crush | Pre-built binaries via aqua registry |
| `npm` | gemini, copilot (`npm:@github/copilot`) | Installed as npm global packages |

## Editing Guidelines

- When adding tools to `mise.toml`, use the correct backend prefix (e.g., `npm:` for npm packages)
- Version specifiers: `latest`, `3.13` (prefix match), `stable` (for rust), or exact like `3.13.2`
- Documentation is bilingual — Chinese files live in `doc/zh/`, English in `doc/en/`; keep both in sync when editing
