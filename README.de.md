# OpenPencil Skill

<p>
  <a href="./README.md">English</a> · <a href="./README.zh.md">简体中文</a> · <a href="./README.zh-TW.md">繁體中文</a> · <a href="./README.ja.md">日本語</a> · <a href="./README.ko.md">한국어</a> · <a href="./README.fr.md">Français</a> · <a href="./README.es.md">Español</a> · <a href="./README.de.md"><b>Deutsch</b></a> · <a href="./README.pt.md">Português</a> · <a href="./README.ru.md">Русский</a>
</p>

LLM-Skill zum Designen mit [OpenPencil](https://github.com/ZSeven-W/openpencil) — lehrt KI-Agenten die Verwendung des `op` CLI, Batch-Design-DSL, MCP-Tools und Design-Best-Practices.

Folgt der [agentskills.io](https://agentskills.io/specification)-Spezifikation.

## Voraussetzungen

Installieren Sie das `op` CLI vor der Nutzung dieses Skills:

```bash
brew install zseven-w/openpencil/op
# Oder unter macOS / Linux:
curl -fsSL https://raw.githubusercontent.com/ZSeven-W/openpencil/main/scripts/install-op.sh | bash
```

```powershell
irm https://raw.githubusercontent.com/ZSeven-W/openpencil/main/scripts/install-op.ps1 | iex
```

Stellen Sie sicher, dass eine OpenPencil-Instanz (Desktop-App oder Webserver) läuft.

## Installation

### Claude Code

```
/plugin marketplace add zseven-w/openpencil-skill
/plugin install openpencil-skill@openpencil-skill
```

### Cursor

```
/add-plugin openpencil-skill
```

### Codex

```
Fetch and follow instructions from https://raw.githubusercontent.com/zseven-w/openpencil-skill/main/.codex/INSTALL.md
```

### OpenCode

```
Fetch and follow instructions from https://raw.githubusercontent.com/zseven-w/openpencil-skill/main/.opencode/INSTALL.md
```

### Gemini CLI

```bash
gemini extensions install https://github.com/zseven-w/openpencil-skill
```

## Enthaltene Inhalte

- `op` CLI-Befehlsreferenz (app control, document ops, nodes, import, export, templates, layout, pages, variables, codegen)
- Batch-Design-DSL und Sandbox-JavaScript-Modus mit vollständigen Beispielen
- PenNode-Schema — alle v0.8.4-Knotentypen einschließlich interaktiver Widgets, Eigenschaften und Füllungen
- 40+ semantische Rollen mit intelligenten Standardwerten
- Designprinzipien — Typografie, Farbe, Abstände, Schatten
- Layout-Engine-Regeln und Größenbestimmung
- Schichtbasierter MCP-Workflow (skeleton → content → refine)
- Codegen-Pipeline (plan → submit → assemble → clean)
- Import-Unterstützung (SVG, HTML-Seiten und Live-URLs, Web-Snapshots, Figma .fig-Dateien)
- Export und Auslieferung — Bilder, PDF und Präsentationen (PowerPoint, eigenständiges HTML)
- Szenenvorlagen und Stil-Assets, einschließlich importierter DESIGN.md-Guides
- Gängige Muster — Navigation, Hero, Karten, Formulare, Footer
- Tabelle häufiger Fehler

## Lizenz

MIT
