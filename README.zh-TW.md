# OpenPencil Skill

<p>
  <a href="./README.md">English</a> · <a href="./README.zh.md">简体中文</a> · <a href="./README.zh-TW.md"><b>繁體中文</b></a> · <a href="./README.ja.md">日本語</a> · <a href="./README.ko.md">한국어</a> · <a href="./README.fr.md">Français</a> · <a href="./README.es.md">Español</a> · <a href="./README.de.md">Deutsch</a> · <a href="./README.pt.md">Português</a> · <a href="./README.ru.md">Русский</a>
</p>

面向 [OpenPencil](https://github.com/ZSeven-W/openpencil) 的 LLM 設計技能 — 教 AI 智慧體使用 `op` CLI、批量設計 DSL、MCP 工具和設計最佳實踐。

遵循 [agentskills.io](https://agentskills.io/specification) 規範。

## 前置要求

使用前需先安裝 `op` CLI：

```bash
brew install zseven-w/openpencil/op
# 或在 macOS / Linux：
curl -fsSL https://raw.githubusercontent.com/ZSeven-W/openpencil/main/scripts/install-op.sh | bash
```

```powershell
irm https://raw.githubusercontent.com/ZSeven-W/openpencil/main/scripts/install-op.ps1 | iex
```

確保有一個正在運行的 OpenPencil 實例（桌面應用或 Web 伺服器）供 `op` 連接。

## 安裝

> **注意：** 各平台安裝方式不同，請選擇下方對應的整合方式。

### Claude Code（插件市場）

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

## 包含內容

- `op` CLI 命令參考（應用控制、文件操作、節點、匯入、佈局、頁面、變數、程式碼生成）
- 批量設計 DSL 與沙箱 JavaScript 模式及完整範例
- PenNode 結構 — v0.8.0 的所有節點類型（含互動式元件）、屬性與填充
- 40+ 語義角色及智慧預設值
- 設計原則 — 字體、配色、間距、陰影
- 佈局引擎規則與尺寸決策
- 分層 MCP 工作流（skeleton → content → refine）
- 程式碼生成管線（plan → submit → assemble → clean）
- 匯入支援（SVG、Figma .fig 檔案）
- 常用模式 — 導覽列、英雄區、卡片、表單、頁尾
- 常見錯誤表

## 授權

MIT
