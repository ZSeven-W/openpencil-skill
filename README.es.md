# OpenPencil Skill

<p>
  <a href="./README.md">English</a> · <a href="./README.zh.md">简体中文</a> · <a href="./README.zh-TW.md">繁體中文</a> · <a href="./README.ja.md">日本語</a> · <a href="./README.ko.md">한국어</a> · <a href="./README.fr.md">Français</a> · <a href="./README.es.md"><b>Español</b></a> · <a href="./README.de.md">Deutsch</a> · <a href="./README.pt.md">Português</a> · <a href="./README.ru.md">Русский</a>
</p>

Habilidad LLM para diseñar con [OpenPencil](https://github.com/ZSeven-W/openpencil) — enseña a agentes IA a usar el CLI `op`, DSL de diseño por lotes, herramientas MCP y mejores prácticas de diseño.

Sigue la especificación [agentskills.io](https://agentskills.io/specification).

## Requisitos previos

Instala el CLI `op` antes de usar esta habilidad:

```bash
brew install zseven-w/openpencil/op
# O en macOS / Linux:
curl -fsSL https://raw.githubusercontent.com/ZSeven-W/openpencil/main/scripts/install-op.sh | bash
```

```powershell
irm https://raw.githubusercontent.com/ZSeven-W/openpencil/main/scripts/install-op.ps1 | iex
```

Asegúrate de tener una instancia de OpenPencil en ejecución (app de escritorio o servidor web).

## Instalación

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

## Contenido incluido

- Referencia de comandos CLI `op` (app control, document ops, nodes, import, layout, pages, variables, codegen)
- DSL de diseño por lotes y modo JavaScript aislado con ejemplos completos
- Esquema PenNode — todos los tipos de nodo de v0.8.0, incluidos widgets interactivos, propiedades y rellenos
- 40+ roles semánticos con valores predeterminados inteligentes
- Principios de diseño — tipografía, color, espaciado, sombras
- Reglas del motor de diseño y dimensionamiento
- Flujo de trabajo MCP en capas (skeleton → content → refine)
- Pipeline de generación de código (plan → submit → assemble → clean)
- Soporte de importación (SVG, archivos Figma .fig)
- Patrones comunes — barra de navegación, hero, tarjetas, formularios, pie de página
- Tabla de errores comunes

## Licencia

MIT
