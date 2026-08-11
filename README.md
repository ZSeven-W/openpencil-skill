# OpenPencil Skill

<p>
  <a href="./README.md"><b>English</b></a> · <a href="./README.zh.md">简体中文</a> · <a href="./README.zh-TW.md">繁體中文</a> · <a href="./README.ja.md">日本語</a> · <a href="./README.ko.md">한국어</a> · <a href="./README.fr.md">Français</a> · <a href="./README.es.md">Español</a> · <a href="./README.de.md">Deutsch</a> · <a href="./README.pt.md">Português</a> · <a href="./README.ru.md">Русский</a>
</p>

LLM skill for designing with [OpenPencil](https://github.com/ZSeven-W/openpencil) — teaches AI agents to use the `op` CLI, batch design DSL, MCP tools, and design best practices.

Follows the [agentskills.io](https://agentskills.io/specification) specification.

## Prerequisites

Install the `op` CLI before using this skill:

```bash
brew install zseven-w/openpencil/op
# Or on macOS / Linux:
curl -fsSL https://raw.githubusercontent.com/ZSeven-W/openpencil/main/scripts/install-op.sh | bash
```

```powershell
irm https://raw.githubusercontent.com/ZSeven-W/openpencil/main/scripts/install-op.ps1 | iex
```

Ensure a running OpenPencil instance (desktop app or web server) for `op` to connect to.

## Installation

> **Note:** Installation differs by platform. Use the matching integration below.

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

- `op` CLI command reference (app control, document ops, nodes, import, export, templates, layout, pages, variables, codegen)
- Batch Design DSL and sandboxed JavaScript mode with full examples
- PenNode schema — all v0.8.4 node types, including interactive widgets, properties, and fills
- 40+ semantic roles with smart defaults
- Design principles — typography, color, spacing, shadows
- Layout engine rules and sizing decisions
- Layered MCP workflow (skeleton → content → refine)
- Codegen pipeline (plan → submit → assemble → clean)
- Import support (SVG, HTML pages and live URLs, web-capture snapshots, Figma .fig files)
- Export and delivery — images, PDF, and presentation decks (PowerPoint, self-contained HTML)
- Scene templates and style assets, including your own imported DESIGN.md guides
- Common patterns — navbar, hero, cards, forms, footer
- Common mistakes table

## License

MIT
