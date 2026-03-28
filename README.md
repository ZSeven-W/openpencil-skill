# OpenPencil Skill

LLM skill for designing with [OpenPencil](https://github.com/ZSeven-W/openpencil) — teaches AI agents to use the `op` CLI, batch design DSL, MCP tools, and design best practices.

## Setup

### Claude Code

Add to your project's `.claude/settings.json`:

```json
{
  "permissions": {
    "allow": ["Bash(op *)"]
  }
}
```

Then reference the skill in your `CLAUDE.md`:

```markdown
## Design Skill

When designing with OpenPencil, follow the guide at:
https://raw.githubusercontent.com/ZSeven-W/openpencil-skill/main/openpencil.md
```

Or clone locally and reference the file path:

```bash
git clone https://github.com/zseven-w/openpencil-skill.git .openpencil-skill
```

```markdown
## Design Skill

See .openpencil-skill/openpencil.md for OpenPencil design guidance.
```

### Cursor / Windsurf

Add to your project rules (`.cursorrules` or `.windsurfrules`):

```
@openpencil-skill/openpencil.md
```

### Other LLM Agents

Include the content of `openpencil.md` in your system prompt or context when working with OpenPencil.

## What's Included

- **`op` CLI reference** — all commands, flags, input methods
- **Batch Design DSL** — full syntax with real-world landing page example
- **PenNode schema** — every node type, property, and fill type
- **Semantic roles** — 40+ roles with smart defaults
- **Design principles** — typography, color, spacing, shadows
- **Layout engine rules** — flexbox model, sizing decisions
- **Layered workflow** — skeleton → content → refine for complex pages
- **Common patterns** — navbar, hero, cards, forms, pricing, stats, footer
- **Quality checklist** — pre-submission validation

## License

MIT
