# OpenPencil Skill

LLM skill for designing with [OpenPencil](https://github.com/ZSeven-W/openpencil) — teaches AI agents to use the `op` CLI, batch design DSL, MCP tools, and design best practices.

Follows the [agentskills.io](https://agentskills.io/specification) specification.

## Setup

### Claude Code

Reference in your `CLAUDE.md`:

```markdown
See .openpencil-skill/SKILL.md for OpenPencil design guidance.
```

### Cursor / Windsurf

Add to project rules (`.cursorrules` / `.windsurfrules`):

```
@openpencil-skill/SKILL.md
```

### Other Agents

Include `SKILL.md` content in your system prompt or context.

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
