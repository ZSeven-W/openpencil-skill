# OpenPencil Skill

<p>
  <a href="./README.md">English</a> · <a href="./README.zh.md">简体中文</a> · <a href="./README.zh-TW.md">繁體中文</a> · <a href="./README.ja.md">日本語</a> · <a href="./README.ko.md">한국어</a> · <a href="./README.fr.md">Français</a> · <a href="./README.es.md"><b>Español</b></a> · <a href="./README.de.md">Deutsch</a> · <a href="./README.pt.md">Português</a> · <a href="./README.ru.md">Русский</a>
</p>

Habilidad LLM para diseñar con [OpenPencil](https://github.com/ZSeven-W/openpencil) — enseña a agentes IA a usar el CLI `op`, DSL de diseño por lotes, herramientas MCP y mejores prácticas de diseño.

Sigue la especificación [agentskills.io](https://agentskills.io/specification).

## Requisitos previos

Instala el CLI `op` antes de usar esta habilidad:

```bash
npm install -g @zseven-w/openpencil
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

- Referencia de comandos CLI `op`
- Sintaxis DSL de diseño por lotes con ejemplo completo de landing page
- Esquema PenNode — todos los tipos de nodo, propiedades, rellenos
- 40+ roles semánticos con valores predeterminados inteligentes
- Principios de diseño — tipografía, color, espaciado, sombras
- Reglas del motor de diseño y dimensionamiento
- Flujo de trabajo MCP en capas (skeleton → content → refine)
- Patrones comunes — barra de navegación, hero, tarjetas, formularios, pie de página
- Tabla de errores comunes

## Licencia

MIT
