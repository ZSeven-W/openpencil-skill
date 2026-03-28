# OpenPencil Skill

<p>
  <a href="./README.md">English</a> · <a href="./README.zh.md">简体中文</a> · <a href="./README.zh-TW.md">繁體中文</a> · <a href="./README.ja.md">日本語</a> · <a href="./README.ko.md">한국어</a> · <a href="./README.fr.md">Français</a> · <a href="./README.es.md">Español</a> · <a href="./README.de.md">Deutsch</a> · <a href="./README.pt.md">Português</a> · <a href="./README.ru.md"><b>Русский</b></a>
</p>

LLM-навык для дизайна с [OpenPencil](https://github.com/ZSeven-W/openpencil) — обучает ИИ-агентов использованию CLI `op`, пакетного DSL дизайна, инструментов MCP и лучших практик дизайна.

Соответствует спецификации [agentskills.io](https://agentskills.io/specification).

## Предварительные требования

Установите CLI `op` перед использованием этого навыка:

```bash
npm install -g @zseven-w/openpencil
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

- Справочник команд CLI `op`
- Синтаксис пакетного DSL дизайна с полным примером лендинга
- Схема PenNode — все типы узлов, свойства, заливки
- 40+ семантических ролей с умными значениями по умолчанию
- Принципы дизайна — типографика, цвет, отступы, тени
- Правила движка компоновки и определение размеров
- Многослойный рабочий процесс MCP (skeleton → content → refine)
- Типовые паттерны — навигация, герой, карточки, формы, подвал
- Таблица распространённых ошибок

## Лицензия

MIT
