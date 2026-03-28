# OpenPencil Skill

<p>
  <a href="./README.md">English</a> · <a href="./README.zh.md">简体中文</a> · <a href="./README.zh-TW.md">繁體中文</a> · <a href="./README.ja.md"><b>日本語</b></a> · <a href="./README.ko.md">한국어</a> · <a href="./README.fr.md">Français</a> · <a href="./README.es.md">Español</a> · <a href="./README.de.md">Deutsch</a> · <a href="./README.pt.md">Português</a> · <a href="./README.ru.md">Русский</a>
</p>

[OpenPencil](https://github.com/ZSeven-W/openpencil) 向けの LLM デザインスキル — AIエージェントに `op` CLI、バッチデザインDSL、MCPツール、デザインのベストプラクティスを教えます。

[agentskills.io](https://agentskills.io/specification) 仕様に準拠。

## 前提条件

使用前に `op` CLI をインストールしてください：

```bash
npm install -g @zseven-w/openpencil
```

`op` が接続するための OpenPencil インスタンス（デスクトップアプリまたはWebサーバー）が実行中であることを確認してください。

## インストール

> **注意：** プラットフォームによりインストール方法が異なります。

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

## 含まれる内容

- `op` CLI コマンドリファレンス
- バッチデザインDSL構文とランディングページの完全な例
- PenNodeスキーマ — 全ノードタイプ、プロパティ、フィル
- 40以上のセマンティックロールとスマートデフォルト
- デザイン原則 — タイポグラフィ、カラー、スペーシング、シャドウ
- レイアウトエンジンのルールとサイジング
- 階層型MCPワークフロー（skeleton → content → refine）
- 一般的なパターン — ナビバー、ヒーロー、カード、フォーム、フッター
- よくあるミスの一覧

## ライセンス

MIT
