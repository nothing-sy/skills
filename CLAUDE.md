# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Claude Code Agent Skills repository for creating and managing reusable skill modules. Skills are invoked via `/skill-name` and can also be triggered automatically based on conditions defined in their frontmatter.

## Creating a New Skill

1. Create a directory under `skills/` with the skill name
2. Create a `skill.md` file with YAML frontmatter:

```markdown
---
name: skill-name
description: Brief description of what this skill does
triggers:
  - Condition when this skill should auto-activate
---

# Skill content and instructions...
```

## Skill Frontmatter Fields

| Field | Description |
|-------|-------------|
| `name` | Skill identifier for `/skill-name` invocation |
| `description` | Short description shown in skill lists |
| `triggers` | List of conditions for automatic activation |

## GitHub Operations

When users mention GitHub operations (PR, Issue, repo management), prefer GitHub MCP services over local git commands. The project has GitHub MCP integration configured.

## Pre-configured Permissions

The `settings.json` allows:
- `gh **` - GitHub CLI commands
- `git **` - Git commands
- `npx **` - NPM package execution
