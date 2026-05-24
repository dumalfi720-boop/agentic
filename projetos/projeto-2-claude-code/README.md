# Project 2 — Claude Code: Professional Agentic Configuration

![Status](https://img.shields.io/badge/status-ativo-brightgreen)
![Claude Code](https://img.shields.io/badge/Claude%20Code-latest-blue)
![Node.js](https://img.shields.io/badge/Node.js-18%2B-green)
![License](https://img.shields.io/badge/licen%C3%A7a-MIT-lightgrey)

> Professional configuration of Claude Code for agentic development — a complete kit with CLAUDE.md, permissions, hooks, skills and examples ready for use in real projects.

---

## What's included```
projeto-2-claude-code/
├── CLAUDE.md                          # Contexto completo do projeto para o Claude
├── .claude/
│   ├── settings.json                  # Permissões, hooks e variáveis de ambiente
│   └── skills/
│       ├── fix-issue/
│       │   └── SKILL.md               # Skill para corrigir issues do GitHub
│       ├── review-pr/
│       │   └── SKILL.md               # Skill para fazer review de Pull Requests
│       └── write-tests/
│           └── SKILL.md               # Skill para gerar testes automaticamente
├── scripts/
│   ├── setup.sh                       # Script de setup automatizado do projeto
│   └── check-config.sh                # Script de verificação da configuração
├── exemplos/
│   ├── mcp-config.json                # Exemplo de configuração de MCP servers
│   └── CLAUDE.md.minimal              # Versão mínima do CLAUDE.md para iniciantes
├── ROTEIRO.md                         # Roteiro de estudo passo a passo
└── README.md                          # Este arquivo
```---

## Prerequisites

Before using this project, make sure you have installed:

- **Node.js 18+** — [nodejs.org](https://nodejs.org)
- **npm** — included with Node.js
- **Claude Code** — installed globally via npm:```bash
npm install -g @anthropic-ai/claude-code
```- **ANTHROPIC_API_KEY** — Anthropic API key configured in the environment:```bash
export ANTHROPIC_API_KEY="sk-ant-..."
# Ou adicione ao seu .bashrc / .zshrc para persistir
```- **gh CLI** (optional, required for issue and PR skills) — [cli.github.com](https://cli.github.com)```bash
# Ubuntu/Debian
sudo apt install gh

# macOS
brew install gh

# Autenticar
gh auth login
```---

## How to use

### 1. Copy the files to your project```bash
# Clonar ou baixar este repositório
git clone https://github.com/seu-usuario/projeto-2-claude-code.git

# Copiar a estrutura .claude para seu projeto
cp -r projeto-2-claude-code/.claude /caminho/do/seu/projeto/

# Copiar o CLAUDE.md (você vai customizar este arquivo)
cp projeto-2-claude-code/CLAUDE.md /caminho/do/seu/projeto/
```Or run the automated setup script:```bash
cd projeto-2-claude-code
chmod +x scripts/setup.sh
./scripts/setup.sh
```### 2. Adjust CLAUDE.md for your context

O`CLAUDE.md`is the most important file. It defines the context that Claude will use in all interactions. Edit it to reflect **your** project:

- Change the technology stack
- Update development, test and build commands
- Define your team's code conventions
- Configure the desired behavior rules
- Document the project architecture

### 3. Check configuration```bash
chmod +x scripts/check-config.sh
./scripts/check-config.sh
```### 4. Start Claude Code```bash
# No diretório do seu projeto
claude
```---

## Description of each file

###`CLAUDE.md`The Claude Code central configuration file. It works like a complete briefing that Claude reads automatically when starting a project.

**What to include:**
- Context and objective of the project
- Technological stack and versions
- Development commands (install, run, test, build)
- Code conventions (naming, formatting, imports)
- Rules of behavior (what you can and cannot do)
- Folder architecture
- Required environment variables

**How to customize:**```bash
# Abrir para edição
code CLAUDE.md  # VS Code
nano CLAUDE.md  # Terminal
```Replace the FastAPI/Python examples with your actual stack. The more detailed the`CLAUDE.md`, the more accurate and useful Claude's behavior will be.

---

###`.claude/settings.json`Controls Claude Code permissions, lifecycle hooks, and environment variables.

**Main Sections:**

-`permissions.allow`— list of commands that Claude can execute without asking for confirmation
-`permissions.deny`— list of blocked commands (will never be executed)
-`hooks.PreToolUse`— scripts run before any tool
-`hooks.PostToolUse`— scripts run after any tool
-`env`— environment variables injected into sessions

**How to customize:**```json
{
  "permissions": {
    "allow": [
      "Bash(npm run test:*)",
      "Bash(npx:*)"
    ],
    "deny": [
      "Bash(rm -rf:*)"
    ]
  }
}
```Adjust the allow/deny defaults according to your stack and security policy.

---

###`.claude/skills/fix-issue/SKILL.md`Skill that automates the complete process of fixing a GitHub issue.

**Use:**`/fix-issue <numero>`Read the issue, analyze the code, create a branch, implement the fix, run the tests and commit — all autonomously.

---

###`.claude/skills/review-pr/SKILL.md`Skill to carry out a structured review of Pull Requests.

**Use:**`/review-pr <numero>`Search for PR with`gh`, analyzes changes, checks test coverage, checks code conventions and produces a structured review comment.

---

###`.claude/skills/write-tests/SKILL.md`Skill to automatically generate unit tests for a file.

**Use:**`/write-tests <caminho/do/arquivo>`Reads the file, identifies all public functions without corresponding tests, creates the test file with adequate coverage and validates it by running pytest.

---

###`scripts/setup.sh`Bash script that automates the initial configuration of Claude Code in a new project.

**What it does:**
- Checks if Claude Code is installed
- Creates the structure`.claude/`if it doesn't exist
- Check if`ANTHROPIC_API_KEY`is configured
- Prints instructions for next steps

**Usage:**```bash
chmod +x scripts/setup.sh
./scripts/setup.sh
```

---

### `scripts/check-config.sh`Diagnostic script that checks if the configuration is correct before starting Claude Code.

**What you check:**
- Existence and validity of the`.claude/settings.json`- Existence and minimum size of the`CLAUDE.md`- Presence of`ANTHROPIC_API_KEY`in the environment

**Usage:**```bash
chmod +x scripts/check-config.sh
./scripts/check-config.sh
```

---

### `exemplos/mcp-config.json`Complete example of configuring MCP (Model Context Protocol) servers to expand the capabilities of Claude Code.

Includes ready-made configurations for: local filesystem, GitHub, Firebase and PostgreSQL.

---

###`exemplos/CLAUDE.md.minimal`Simplified version of`CLAUDE.md`, ideal for those just starting out or for small projects. Contains only the essential sections: context, commands and basic rules.

---

## Skills available

###`/fix-issue <numero>`Fixes a GitHub issue from end to end:```bash
# Dentro do Claude Code
/fix-issue 123
```Claude will:
1. Read the issue details with`gh issue view`2. Analyze related code
3. Create a branch`fix/issue-123-descricao`4. Implement the fix
5. Run the tests
6. Commit with reference to the issue

###`/review-pr <numero>`Perform a structured review of a PR:```bash
/review-pr 45
```

### `/write-tests <arquivo>`Generates tests for an existing file:```bash
/write-tests app/services/task_service.py
```---

### How to create new skills

A skill is a simple Markdown file with structured instructions. To create a new one:

**1. Create the directory and file:**```bash
mkdir -p .claude/skills/minha-skill
touch .claude/skills/minha-skill/SKILL.md
```**2. Structure of`SKILL.md`:**

```markdown
# Skill: minha-skill

## Descrição
O que esta skill faz em 2-3 frases.

## Como Usar
`/minha-skill <argumento>`

## Passos de Execução
### Passo 1 — ...
### Passo 2 — ...

## Ferramentas Disponíveis
| Ferramenta | Uso |
|------------|-----|
| Bash       | ... |
| Read       | ... |

## Saída Esperada
O que o Claude deve apresentar ao final.
```**3. Use the skill:**```bash
# No Claude Code
/minha-skill argumento
```Claude will read the`SKILL.md`corresponding and follow the defined instructions.

---

## Advanced usage tips

### Non-interactive mode for CI/CD```bash
# Executar um comando sem abrir o chat interativo
claude --print "Rode os testes e mostre o resultado"

# Usar em pipelines
echo "Analise os erros de lint" | claude --print
```### Configuration per project vs global```bash
# Configuração global (afeta todos os projetos)
~/.claude/settings.json

# Configuração por projeto (sobrescreve a global)
.claude/settings.json
```### Hooks for automation

Use hooks to automatically perform actions. Practical examples:```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "hooks": [{
          "type": "command",
          "command": "npm run lint --silent 2>&1 | head -20"
        }]
      }
    ]
  }
}
```### Multiple contexts with hierarchical CLAUDE.md

You may have`CLAUDE.md`in subdirectories to add specific context:```
projeto/
├── CLAUDE.md              # Contexto geral do projeto
├── frontend/
│   └── CLAUDE.md          # Contexto específico do frontend (React, etc.)
└── backend/
    └── CLAUDE.md          # Contexto específico do backend
```### Safe environment variables

Never place keys directly into`settings.json`. Use references to environment variables:```json
{
  "env": {
    "ANTHROPIC_API_KEY": "${ANTHROPIC_API_KEY}",
    "DATABASE_URL": "${DATABASE_URL}"
  }
}
```### Debug Claude's behavior

If Claude is not following expected instructions:

1. Check that the`CLAUDE.md`is in the project root directory
2. Check if the`settings.json`is valid JSON:`python3 -m json.tool .claude/settings.json`3. Use command`/config`inside Claude Code to see the active settings
4. Make instructions more specific and explicit in the`CLAUDE.md`---

## Additional features

- [Official Claude Code documentation](https://docs.anthropic.com/claude-code)
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io)
- [GitHub CLI (gh)](https://cli.github.com/manual)
- [ROTEIRO.md](./ROTEIRO.md) — step-by-step study guide included in this project