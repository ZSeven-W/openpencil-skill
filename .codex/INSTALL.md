# Installing OpenPencil Skill for Codex

## Prerequisites

- Git

## Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/zseven-w/openpencil-skill.git ~/.codex/openpencil-skill
   ```

2. **Create the skills symlink:**
   ```bash
   mkdir -p ~/.agents/skills
   ln -s ~/.codex/openpencil-skill/skills ~/.agents/skills/openpencil-skill
   ```

   **Windows (PowerShell):**
   ```powershell
   New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
   cmd /c mklink /J "$env:USERPROFILE\.agents\skills\openpencil-skill" "$env:USERPROFILE\.codex\openpencil-skill\skills"
   ```

3. **Restart Codex** to discover the skills.

## Updating

```bash
cd ~/.codex/openpencil-skill && git pull
```

## Uninstalling

```bash
rm ~/.agents/skills/openpencil-skill
rm -rf ~/.codex/openpencil-skill
```
