# Project 4 — Agentic with Codex CLI

![Status](https://img.shields.io/badge/status-ativo-brightgreen)
![Node](https://img.shields.io/badge/node-%3E%3D18-blue)
![License](https://img.shields.io/badge/licen%C3%A7a-MIT-green)
![OpenAI](https://img.shields.io/badge/powered%20by-OpenAI%20Codex-412991)

> OpenAI Codex CLI configuration for agentic development

---

## What is this project

This project contains everything you need to configure the **OpenAI Codex CLI** in your development environment and start using AI agentically — that is, letting the agent perform real tasks in your code with different levels of autonomy.

---

## What's included

| Archive | Description |
|---|---|
|`AGENTS.md`| Context guide automatically read by Codex every session |
|`.codex/config.toml`| Agent behavior configuration (model, mode, timeouts) |
|`scripts/setup.sh`| Environment installation and configuration script |
|`scripts/run-headless.sh`| Perform tasks without human interaction (CI/CD mode) |
|`exemplos/github-actions.yml`| Complete automatic code review workflow with GitHub Actions |
|`.env.example`| Environment variables template |
|`COMO_EXECUTAR.txt`| Simple Language Beginner's Guide |
|`ROTEIRO.md`| Step-by-step roadmap to master Codex CLI |

---

## Prerequisites

Before you start, make sure you have:

- **Node.js 18 or higher** — [download at nodejs.org](https://nodejs.org)
- **npm** — comes bundled with Node.js
- **OpenAI API Key** — get it from [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
- **Git** — to clone repositories and use PR review mode

To check if you already have Node.js installed:```bash
node --version
# Deve mostrar v18.0.0 ou superior
```---

## Installation```bash
# Instalar o Codex CLI globalmente
npm install -g @openai/codex

# Verificar se instalou corretamente
codex --version

# Configurar a API key (escolha uma das opções abaixo)

# Opção 1: Variável de ambiente temporária (dura apenas na sessão atual)
export OPENAI_API_KEY="sk-..."

# Opção 2: Adicionar permanentemente ao seu shell (recomendado)
echo 'export OPENAI_API_KEY="sk-..."' >> ~/.bashrc
source ~/.bashrc

# Opção 3: Arquivo .env no projeto (para times — nunca commitar com o valor real)
cp .env.example .env
# Edite o arquivo .env com sua chave
```---

## How to use the files in this project

### Option 1: Use as template (recommended)

Copy the relevant files for your project:```bash
# Copiar o AGENTS.md (adapte para o contexto do seu projeto)
cp AGENTS.md /caminho/do/seu/projeto/AGENTS.md

# Copiar a configuração do Codex
mkdir -p /caminho/do/seu/projeto/.codex
cp .codex/config.toml /caminho/do/seu/projeto/.codex/config.toml

# Copiar os scripts
cp scripts/*.sh /caminho/do/seu/projeto/scripts/
chmod +x /caminho/do/seu/projeto/scripts/*.sh
```### Option 2: Run the setup script```bash
# O script verifica dependências e configura tudo automaticamente
bash scripts/setup.sh
```### Option 3: Use directly in this directory

If you want to test without copying to another project, just run the Codex here:```bash
codex "explique a estrutura do AGENTS.md deste projeto"
```---

## The 3 approval modes

The Codex CLI has three levels of autonomy. Choose depending on how much you trust the agent for each task:

### Mode 1:`suggest`(Suggest)

The agent **reads the code and suggests changes**, but does not execute anything. You see the suggestions and decide what to do.```bash
codex --approval-mode suggest "adicione tratamento de erro na função authenticate"
```**When to use:** Code review, explore possibilities, learn what the agent would do. Ideal for beginners or high-risk moves.

---

### Mode 2:`auto-edit`(Edit with approval)

The agent **can edit files**, but needs your approval to execute commands in the terminal (such as running tests, installing packages, etc.).```bash
codex --approval-mode auto-edit "refatore o módulo de autenticação para usar async/await"
```**When to use:** Refactorings, creation of new files, small features. The most common flow in everyday life.

---

### Mode 3:`full-auto`(Fully automatic)

The agent **has complete autonomy**: edits files, executes commands, runs tests. Works without asking for confirmation.```bash
codex --approval-mode full-auto "crie testes unitários para o UserService"
```**When to use:** Well-defined repetitive tasks, CI/CD pipelines, boilerplate code generation. Use with caution in production code.

---

## Claude Code vs Codex CLI — when to use each

| Criterion | Claude Code | Codex CLI |
|---|---|---|
| **Company** | Anthropic | OpenAI |
| **Base model** | Claude (Sonnet/Opus) | GPT-4o/o3 |
| **Best for** | Complex reasoning, architecture, long explanations | Code generation, refactorings, well-defined tasks |
| **Project context** |`CLAUDE.md` | `AGENTS.md`|
| **Use in CI/CD** | Possible but less native | Excellent (headless mode) |
| **Cost** | By token (Claude API) | By token (OpenAI API) |
| **Interface** | Interactive CLI | Interactive + headless CLI |
| **GitHub Integration** | Manual | Native GitHub Actions |
| **Whenever you prefer** | Architectural discussions, complex debugging, code analysis | Automation, mass generation, CI/CD pipelines |

**Rule of thumb:** Use Claude Code when you need to _think together_ with the agent. Use Codex when you need to _perform_ a well-defined task.

---

## Productivity tips

### 1. Be specific in instructions```bash
# Ruim — muito vago
codex "melhore o código"

# Bom — especifico e acionável
codex "refatore a função createUser em src/modules/users/user.service.js para usar early returns e reduzir aninhamento"
```### 2. Use AGENTS.md to provide context

The more detailed your`AGENTS.md`, the less you need to explain in each session. Document:
- Technological stack
- Naming conventions
- Test and lint commands
- What the agent should NEVER do

### 3. Start with`suggest`, evolved into`auto-edit`For new or unfamiliar tasks, always start in`suggest`. Once you gain confidence in what the agent does, switch to`auto-edit`.

### 4. Combine with Git

Before using the mode`full-auto`, create a branch:```bash
git checkout -b feature/ai-refactor
codex --approval-mode full-auto "refatore todos os controllers para usar o padrão X"
git diff  # Revise as mudanças
```### 5. Use headless mode for repetitive tasks```bash
# Gerar testes para vários arquivos
for file in src/modules/*/service.js; do
  ./scripts/run-headless.sh "escreva testes unitários para $file"
done
```### 6. Save long sessions

If a session is interrupted, Codex maintains project context via`AGENTS.md`. Describe what you were doing at the start of the new session:```bash
codex "continuando de onde paramos: estávamos refatorando o módulo de auth. O service já foi atualizado, falta o controller e os testes"
```---

## Project structure```
projeto-4-codex/
├── AGENTS.md                  # Contexto do projeto para o Codex
├── .codex/
│   └── config.toml            # Configuração do agente
├── scripts/
│   ├── setup.sh               # Instalação e configuração
│   └── run-headless.sh        # Execução sem interação
├── exemplos/
│   └── github-actions.yml     # Workflow de CI/CD com Codex
├── .env.example               # Template de variáveis
├── README.md                  # Este arquivo
├── ROTEIRO.md                 # Guia passo a passo
└── COMO_EXECUTAR.txt          # Guia para iniciantes
```---

## Resources and documentation

- [Official Codex CLI documentation](https://github.com/openai/codex)
- [OpenAI API Reference](https://platform.openai.com/docs)
- [AGENTS.md examples](https://github.com/openai/codex/tree/main/examples)
- [GitHub Actions with OpenAI](https://docs.github.com/en/actions)

---

*Part of the agentic development course — Project 4 of 8*