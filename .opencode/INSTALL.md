# Installing OpenPencil Skill for OpenCode

## Prerequisites

- [OpenCode.ai](https://opencode.ai) installed

## Installation

Add to the `plugin` array in your `opencode.json` (global or project-level):

```json
{
  "plugin": ["openpencil-skill@git+https://github.com/zseven-w/openpencil-skill.git"]
}
```

Restart OpenCode. The plugin auto-installs and registers all skills.

## Updating

Restarts OpenCode to pull the latest version automatically.

To pin a specific version:

```json
{
  "plugin": ["openpencil-skill@git+https://github.com/zseven-w/openpencil-skill.git#v0.7.0"]
}
```

## Uninstalling

Remove the plugin line from `opencode.json` and restart OpenCode.
