# OpenPencil Skill

<p>
  <a href="./README.md">English</a> · <a href="./README.zh.md">简体中文</a> · <a href="./README.zh-TW.md">繁體中文</a> · <a href="./README.ja.md">日本語</a> · <a href="./README.ko.md">한국어</a> · <a href="./README.fr.md">Français</a> · <a href="./README.es.md">Español</a> · <a href="./README.de.md">Deutsch</a> · <a href="./README.pt.md"><b>Português</b></a> · <a href="./README.ru.md">Русский</a>
</p>

Habilidade LLM para design com [OpenPencil](https://github.com/ZSeven-W/openpencil) — ensina agentes IA a usar o CLI `op`, DSL de design em lote, ferramentas MCP e melhores práticas de design.

Segue a especificação [agentskills.io](https://agentskills.io/specification).

## Pré-requisitos

Instale o CLI `op` antes de usar esta habilidade:

```bash
brew install zseven-w/openpencil/op
# Ou no macOS / Linux:
curl -fsSL https://raw.githubusercontent.com/ZSeven-W/openpencil/main/scripts/install-op.sh | bash
```

```powershell
irm https://raw.githubusercontent.com/ZSeven-W/openpencil/main/scripts/install-op.ps1 | iex
```

Certifique-se de que uma instância do OpenPencil (app desktop ou servidor web) está em execução.

## Instalação

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

## Conteudo incluido

- Referencia de comandos CLI `op` (app control, document ops, nodes, import, export, templates, layout, pages, variables, codegen)
- DSL de design em lote e modo JavaScript isolado com exemplos completos
- Schema PenNode — todos os tipos de nó da v0.8.4, incluindo widgets interativos, propriedades e preenchimentos
- 40+ roles semanticos com valores padrao inteligentes
- Principios de design — tipografia, cor, espacamento, sombras
- Regras do motor de layout e dimensionamento
- Fluxo de trabalho MCP em camadas (skeleton → content → refine)
- Pipeline de geracao de codigo (plan → submit → assemble → clean)
- Suporte a importacao (SVG, paginas HTML e URLs ao vivo, capturas web, arquivos Figma .fig)
- Exportacao e entrega — imagens, PDF e apresentacoes (PowerPoint, HTML autocontido)
- Modelos de cena e recursos de estilo, incluindo seus guias DESIGN.md importados
- Padroes comuns — barra de navegacao, hero, cards, formularios, rodape
- Tabela de erros comuns

## Licenca

MIT
