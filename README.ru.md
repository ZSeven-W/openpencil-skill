# OpenPencil Skill

<p>
  <a href="./README.md">English</a> · <a href="./README.zh.md">简体中文</a> · <a href="./README.zh-TW.md">繁體中文</a> · <a href="./README.ja.md">日本語</a> · <a href="./README.ko.md">한국어</a> · <a href="./README.fr.md">Français</a> · <a href="./README.es.md">Español</a> · <a href="./README.de.md">Deutsch</a> · <a href="./README.pt.md">Português</a> · <a href="./README.ru.md"><b>Русский</b></a>
</p>

LLM-навык для дизайна с [OpenPencil](https://github.com/ZSeven-W/openpencil) — обучает ИИ-агентов использованию CLI `op`, пакетного DSL дизайна, инструментов MCP и лучших практик дизайна.

Соответствует спецификации [agentskills.io](https://agentskills.io/specification).

## Предварительные требования

Установите CLI `op` перед использованием этого навыка:

```bash
brew install zseven-w/openpencil/op
# Или в macOS / Linux:
curl -fsSL https://raw.githubusercontent.com/ZSeven-W/openpencil/main/scripts/install-op.sh | bash
```

```powershell
irm https://raw.githubusercontent.com/ZSeven-W/openpencil/main/scripts/install-op.ps1 | iex
```

Убедитесь, что экземпляр OpenPencil (десктопное приложение или веб-сервер) запущен.

## Установка

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

## Содержание

- Справочник команд CLI `op` (app control, document ops, nodes, import, layout, pages, variables, codegen)
- Пакетный DSL дизайна и изолированный режим JavaScript с полными примерами
- Схема PenNode — все типы узлов v0.8.0, включая интерактивные виджеты, свойства и заливки
- 40+ семантических ролей с умными значениями по умолчанию
- Принципы дизайна — типографика, цвет, отступы, тени
- Правила движка компоновки и определение размеров
- Многослойный рабочий процесс MCP (skeleton → content → refine)
- Конвейер генерации кода (plan → submit → assemble → clean)
- Поддержка импорта (SVG, файлы Figma .fig)
- Типовые паттерны — навигация, герой, карточки, формы, подвал
- Таблица распространённых ошибок

## Лицензия

MIT
