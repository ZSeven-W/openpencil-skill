# OpenPencil Skill

<p>
  <a href="./README.md">English</a> · <a href="./README.zh.md">简体中文</a> · <a href="./README.zh-TW.md">繁體中文</a> · <a href="./README.ja.md">日本語</a> · <a href="./README.ko.md"><b>한국어</b></a> · <a href="./README.fr.md">Français</a> · <a href="./README.es.md">Español</a> · <a href="./README.de.md">Deutsch</a> · <a href="./README.pt.md">Português</a> · <a href="./README.ru.md">Русский</a>
</p>

[OpenPencil](https://github.com/ZSeven-W/openpencil)용 LLM 디자인 스킬 — AI 에이전트에게 `op` CLI, 배치 디자인 DSL, MCP 도구 및 디자인 모범 사례를 교육합니다.

[agentskills.io](https://agentskills.io/specification) 사양을 따릅니다.

## 사전 요구 사항

사용 전 `op` CLI를 설치하세요:

```bash
npm install -g @zseven-w/openpencil
```

`op`가 연결할 OpenPencil 인스턴스(데스크톱 앱 또는 웹 서버)가 실행 중인지 확인하세요.

## 설치

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

## 포함 내용

- `op` CLI 명령 참조
- 배치 디자인 DSL 구문 및 전체 랜딩 페이지 예제
- PenNode 스키마 — 모든 노드 유형, 속성, 채우기
- 40개 이상의 시맨틱 역할과 스마트 기본값
- 디자인 원칙 — 타이포그래피, 색상, 간격, 그림자
- 레이아웃 엔진 규칙 및 크기 결정
- 계층형 MCP 워크플로 (skeleton → content → refine)
- 일반 패턴 — 내비바, 히어로, 카드, 폼, 푸터
- 일반적인 실수 표

## 라이선스

MIT
