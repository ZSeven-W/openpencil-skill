# OpenPencil Skill

LLM skill for designing with [OpenPencil](https://github.com/ZSeven-W/openpencil) — teaches AI agents to use the `op` CLI, batch design DSL, MCP tools, and design best practices.

Follows the [agentskills.io](https://agentskills.io/specification) specification.

## Installation

> **Note:** Installation differs by platform. Claude Code and Cursor have built-in plugin systems. Codex and OpenCode require manual setup.

### Claude Code (Plugin Marketplace)

Register the marketplace, then install the plugin:

```
/plugin marketplace add zseven-w/openpencil-skill
/plugin install openpencil-skill@openpencil-skill
```

### Cursor

In Cursor Agent chat:

```
/add-plugin openpencil-skill
```

Or search for "openpencil" in the plugin marketplace.

### Codex

Tell Codex:

```
Fetch and follow instructions from https://raw.githubusercontent.com/zseven-w/openpencil-skill/main/.codex/INSTALL.md
```

### OpenCode

Tell OpenCode:

```
Fetch and follow instructions from https://raw.githubusercontent.com/zseven-w/openpencil-skill/main/.opencode/INSTALL.md
```

### Gemini CLI

```bash
gemini extensions install https://github.com/zseven-w/openpencil-skill
```

To update:

```bash
gemini extensions update openpencil-skill
```

## Verify Installation

Start a new session and ask to design something with OpenPencil (e.g., "design a landing page using op CLI"). The agent should automatically use the skill to generate PenNode JSON via the `op` CLI or MCP tools.

## What's Included

- `op` CLI command reference
- Batch Design DSL syntax with full landing page example
- PenNode schema — all node types, properties, fills
- 40+ semantic roles with smart defaults
- Design principles — typography, color, spacing, shadows
- Layout engine rules and sizing decisions
- Layered MCP workflow (skeleton → content → refine)
- Common patterns — navbar, hero, cards, forms, footer
- Common mistakes table

## License

MIT
