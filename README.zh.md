# OpenPencil Skill

<p>
  <a href="./README.md">English</a> · <a href="./README.zh.md"><b>简体中文</b></a> · <a href="./README.zh-TW.md">繁體中文</a> · <a href="./README.ja.md">日本語</a> · <a href="./README.ko.md">한국어</a> · <a href="./README.fr.md">Français</a> · <a href="./README.es.md">Español</a> · <a href="./README.de.md">Deutsch</a> · <a href="./README.pt.md">Português</a> · <a href="./README.ru.md">Русский</a>
</p>

面向 [OpenPencil](https://github.com/ZSeven-W/openpencil) 的 LLM 设计技能 — 教 AI 智能体使用 `op` CLI、批量设计 DSL、MCP 工具和设计最佳实践。

遵循 [agentskills.io](https://agentskills.io/specification) 规范。

## 前置要求

使用前需先安装 `op` CLI：

```bash
npm install -g @zseven-w/openpencil
```

确保有一个正在运行的 OpenPencil 实例（桌面应用或 Web 服务器）供 `op` 连接。

## 安装

> **注意：** 各平台安装方式不同。Claude Code 和 Cursor 有内置插件系统，Codex 和 OpenCode 需手动配置。

### Claude Code（插件市场）

注册市场，然后安装插件：

```
/plugin marketplace add zseven-w/openpencil-skill
/plugin install openpencil-skill@openpencil-skill
```

### Cursor

在 Cursor Agent 聊天中：

```
/add-plugin openpencil-skill
```

或在插件市场搜索 "openpencil"。

### Codex

告诉 Codex：

```
Fetch and follow instructions from https://raw.githubusercontent.com/zseven-w/openpencil-skill/main/.codex/INSTALL.md
```

### OpenCode

告诉 OpenCode：

```
Fetch and follow instructions from https://raw.githubusercontent.com/zseven-w/openpencil-skill/main/.opencode/INSTALL.md
```

### Gemini CLI

```bash
gemini extensions install https://github.com/zseven-w/openpencil-skill
```

更新：

```bash
gemini extensions update openpencil-skill
```

## 验证安装

启动新会话，要求使用 OpenPencil 设计（例如 "用 op CLI 设计一个落地页"）。智能体应自动调用技能，通过 `op` CLI 或 MCP 工具生成 PenNode JSON。

## 包含内容

- `op` CLI 命令参考
- 批量设计 DSL 语法及完整落地页示例
- PenNode 模式 — 所有节点类型、属性、填充
- 40+ 语义角色及智能默认值
- 设计原则 — 字体、配色、间距、阴影
- 布局引擎规则与尺寸决策
- 分层 MCP 工作流（skeleton → content → refine）
- 常用模式 — 导航栏、英雄区、卡片、表单、页脚
- 常见错误表

## 许可证

MIT
