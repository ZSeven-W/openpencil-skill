# OpenPencil Skill

<p>
  <a href="./README.md">English</a> · <a href="./README.zh.md">简体中文</a> · <a href="./README.zh-TW.md">繁體中文</a> · <a href="./README.ja.md">日本語</a> · <a href="./README.ko.md">한국어</a> · <a href="./README.fr.md">Français</a> · <a href="./README.es.md">Español</a> · <a href="./README.de.md"><b>Deutsch</b></a> · <a href="./README.pt.md">Português</a> · <a href="./README.ru.md">Русский</a>
</p>

LLM-Skill zum Designen mit [OpenPencil](https://github.com/ZSeven-W/openpencil) — lehrt KI-Agenten die Verwendung des `op` CLI, Batch-Design-DSL, MCP-Tools und Design-Best-Practices.

Folgt der [agentskills.io](https://agentskills.io/specification)-Spezifikation.

## Voraussetzungen

Installieren Sie das `op` CLI vor der Nutzung dieses Skills:

```bash
npm install -g @zseven-w/openpencil
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

- `op` CLI-Befehlsreferenz
- Batch-Design-DSL-Syntax mit vollständigem Landing-Page-Beispiel
- PenNode-Schema — alle Knotentypen, Eigenschaften, Füllungen
- 40+ semantische Rollen mit intelligenten Standardwerten
- Designprinzipien — Typografie, Farbe, Abstände, Schatten
- Layout-Engine-Regeln und Größenbestimmung
- Schichtbasierter MCP-Workflow (skeleton → content → refine)
- Gängige Muster — Navigation, Hero, Karten, Formulare, Footer
- Tabelle häufiger Fehler

## Lizenz

MIT
