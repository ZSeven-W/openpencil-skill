# OpenPencil Skill

<p>
  <a href="./README.md">English</a> · <a href="./README.zh.md">简体中文</a> · <a href="./README.zh-TW.md">繁體中文</a> · <a href="./README.ja.md">日本語</a> · <a href="./README.ko.md">한국어</a> · <a href="./README.fr.md"><b>Français</b></a> · <a href="./README.es.md">Español</a> · <a href="./README.de.md">Deutsch</a> · <a href="./README.pt.md">Português</a> · <a href="./README.ru.md">Русский</a>
</p>

Compétence LLM pour le design avec [OpenPencil](https://github.com/ZSeven-W/openpencil) — enseigne aux agents IA l'utilisation du CLI `op`, du DSL de design par lots, des outils MCP et des meilleures pratiques de design.

Conforme à la spécification [agentskills.io](https://agentskills.io/specification).

## Prérequis

Installez le CLI `op` avant d'utiliser cette compétence :

```bash
npm install -g @zseven-w/openpencil
```

Assurez-vous qu'une instance OpenPencil (application de bureau ou serveur web) est en cours d'exécution.

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

## Contenu inclus

- Référence des commandes CLI `op`
- Syntaxe DSL de design par lots avec exemple complet de landing page
- Schéma PenNode — tous les types de nœuds, propriétés, remplissages
- 40+ rôles sémantiques avec valeurs par défaut intelligentes
- Principes de design — typographie, couleurs, espacement, ombres
- Règles du moteur de mise en page et dimensionnement
- Workflow MCP en couches (skeleton → content → refine)
- Modèles courants — barre de navigation, héro, cartes, formulaires, pied de page
- Tableau des erreurs courantes

## Licence

MIT
