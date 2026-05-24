# Study Guide — Mastering the Claude Code

> Step-by-step guide to go from scratch to advanced agentic development with Claude Code. Each step has an estimated duration and practical exercises.

---

## Overview

| Step | Theme | Estimated Time |
|-------|----------------------------------|----------------|
| 1 | Install and authenticate | 10 min |
| 2 | Create your first CLAUDE.md | 30 min |
| 3 | Configure permissions | 20 min |
| 4 | Create your first hook | 15 min |
| 5 | Use the skill /fix-issue | 30 min |
| 6 | Create your own skill | 45 min |
| 7 | Configure MCP servers | 1h |
| 8 | Multi-Agent Challenge | 3h |
| **Total** |                               | **~6h** |

---

## Step 1 — Install and authenticate Claude Code

**Estimated time: 10 minutes**

### Objectives
- Have Claude Code installed and working
- Authenticate with the Anthropic API
- Execute your first command

### Prerequisites
- Node.js 18+ installed
- Anthropic account with active API key

### Instructions

**1.1. Install Claude Code via npm:**```bash
npm install -g @anthropic-ai/claude-code
```Check if it was installed correctly:```bash
claude --version
```**1.2. Configure the API key:**```bash
# Exportar a variável de ambiente
export ANTHROPIC_API_KEY="sk-ant-api..."

# Para persistir entre sessões, adicione ao seu shell profile:
echo 'export ANTHROPIC_API_KEY="sk-ant-api..."' >> ~/.bashrc
source ~/.bashrc
```**1.3. Start Claude Code in any directory:**```bash
mkdir ~/meu-primeiro-projeto
cd ~/meu-primeiro-projeto
claude
```**1.4. Your first command:**

Inside Claude Code, ask something simple:```
Crie um arquivo hello.py que imprime "Olá, Claude Code!" e mostre o conteúdo.
```### Checkpoint
- [ ]`claude --version`returns the installed version
- [ ] Claude Code opens and responds to commands
- [ ] First file created successfully

---

## Step 2 — Create your first CLAUDE.md

**Estimated time: 30 minutes**

### Objectives
- Understand the function and importance of CLAUDE.md
- Create a customized CLAUDE.md for a real project
- Check that Claude uses the defined context

### What is CLAUDE.md

CLAUDE.md is a Markdown file in the root of the project that Claude automatically reads when starting. It works like a briefing: it defines the context, rules and behavior expected from Claude for that specific project.

Without CLAUDE.md, Claude works "blindly". With it, Claude behaves like an experienced developer on your team, knowing the stack, conventions and project rules.

### Instructions

**2.1. Open a real project or create a new one:**```bash
# Usando um projeto existente
cd /caminho/do/seu/projeto

# Ou criando um novo projeto de exemplo
mkdir ~/projeto-estudo && cd ~/projeto-estudo
```**2.2. Use the minimal template as a starting point:**```bash
# Copiar o template mínimo deste projeto
cp exemplos/CLAUDE.md.minimal ~/projeto-estudo/CLAUDE.md
```**2.3. Edit CLAUDE.md to reflect YOUR project:**

Open the file and replace the placeholders:
-`[Nome do Projeto]`→ real name of your project
-`[linguagem/framework]`→ your stack (e.g. Python/FastAPI, Node/Express, Go)
- Dev/test commands → the actual commands for your project
- Rules of behavior → your team’s rules

**2.4. Test whether Claude uses context:**

Start Claude Code from the directory and ask:```
Qual é a linguagem principal deste projeto e como eu rodo os testes?
```Claude should respond based on his CLAUDE.md.

### Tips for an effective CLAUDE.md

- **Be specific about commands:** instead of "run tests", write`pytest tests/ -v`- **Define deny rules:** "never do git push --force", "never delete files without committing"
- **Document the architecture:** a folder diagram helps a lot
- **Include code examples:** show, don't tell — examples of how the code should be written
- **Update regularly:** CLAUDE.md must evolve along with the project

### Checkpoint
- [ ] CLAUDE.md created with real project context
- [ ] Claude responds using the context of CLAUDE.md
- [ ] Claude uses the correct commands when performing tasks

---

## Step 3 — Configure permissions in settings.json

**Estimated time: 20 minutes**

### Objectives
- Understand the Claude Code permissions system
- Configure a security policy for the project
- Test allow and deny behaviors

### What is settings.json

The file`.claude/settings.json`controls what Claude can and cannot do in your project. It defines:

- **`permissions.allow`** — commands that Claude executes without asking for confirmation
- **`permissions.deny`** — commands that Claude will never execute, even if asked

### Instructions

**3.1. Create the .claude structure:**```bash
mkdir -p .claude
```**3.2. Create the initial settings.json:**```json
{
  "permissions": {
    "allow": [],
    "deny": [
      "Bash(rm -rf:*)",
      "Bash(sudo:*)",
      "Bash(git push --force:*)"
    ]
  }
}
```**3.3. Add specific permissions to your stack:**

For a Python/pytest project:```json
{
  "permissions": {
    "allow": [
      "Bash(pytest:*)",
      "Bash(python:*)",
      "Bash(pip install:*)",
      "Bash(git add:*)",
      "Bash(git commit:*)",
      "Bash(git checkout:*)"
    ],
    "deny": [
      "Bash(rm -rf:*)",
      "Bash(sudo:*)",
      "Bash(git push --force:*)",
      "Bash(DROP TABLE:*)"
    ]
  }
}
```For a Node.js project:```json
{
  "permissions": {
    "allow": [
      "Bash(npm run:*)",
      "Bash(npx:*)",
      "Bash(node:*)"
    ]
  }
}
```**3.4. Test permissions:**

In Claude Code, try running a blocked command:```
Execute: rm -rf /tmp/teste
```Claude must refuse or ask for confirmation.

**3.5. Check with the checking script:**```bash
./scripts/check-config.sh
```### Good security practices

- Use the principle of least privilege: only add what is necessary
- Always block`rm -rf`, `sudo` e `git push --force`- Block bank commands that are destructive:`DROP`, `DELETE FROM`without WHERE
- For production projects, be even more restrictive

### Checkpoint
- [ ]`.claude/settings.json`created with configured permissions
- [ ] Claude refuses to execute blocked commands
- [ ] Claude executes allowed commands without asking for extra confirmation

---

## Step 4 — Create your first hook

**Estimated time: 15 minutes**

### Objectives
- Understand Claude Code's hook system
- Create a PreToolUse hook and a PostToolUse hook
- See hooks being executed in real time

### What are hooks

Hooks are scripts that run automatically at specific points in the Claude Code lifecycle:

- **`PreToolUse`** — executed BEFORE any tool (Bash, Edit, Write, Read, etc.)
- **`PostToolUse`** — run AFTER any tool

Hooks receive information about the tool being used via stdin as JSON.

### Instructions

**4.1. Add hooks to settings.json:**```json
{
  "permissions": { ... },
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "echo '[PRE-HOOK] Executando comando Bash: verificando...'"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "echo '[POST-HOOK] Arquivo editado! Lembre de rodar os testes.'"
          }
        ]
      }
    ]
  }
}
```**4.2. Practical hook: run lint after editing Python files:**```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "if command -v ruff &>/dev/null; then ruff check --quiet .; fi"
          }
        ]
      }
    ]
  }
}
```**4.3. Test the hook:**

In Claude Code, ask to edit a file:```
Crie um arquivo teste.py com uma função simples
```Observe the hook output in the terminal.

**4.4. Hook for audit log:**```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": ".*",
        "hooks": [
          {
            "type": "command",
            "command": "echo \"[$(date '+%H:%M:%S')] Tool chamada\" >> .claude/audit.log"
          }
        ]
      }
    ]
  }
}
```### Checkpoint
- [ ] Hook PreToolUse created and working
- [ ] Hook PostToolUse created and working
- [ ] Hook logs appear in the terminal

---

## Step 5 — Use the /fix-issue skill with a real GitHub issue

**Estimated time: 30 minutes**

### Objectives
- Use the fix-issue skill in a real repository
- Observe Claude executing a complete agentic flow
- Learn from Claude's behavior during execution

### Prerequisites
- GitHub CLI (`gh`) installed and authenticated:`gh auth login`- Have a repository with at least one open issue
- Be in the repository directory

### Instructions

**5.1. Verify GitHub CLI authentication:**```bash
gh auth status
gh repo view  # deve mostrar informações do repositório atual
```**5.2. List open issues in the repository:**```bash
gh issue list
```Choose a simple issue to start with — misbehaving bugs or one-off improvements work better than major refactorings.

**5.3. Execute the skill:**

From Claude Code:```
/fix-issue <numero-da-issue>
```**5.4. Observe the execution flow:**

Pay attention to each step Claude takes:
1. Search for the issue with`gh issue view`2. Analyze the code with Grep and Read
3. Create a branch with correct naming
4. Implement the fix
5. Run the tests
6. Commit with reference to the issue

**5.5. After execution, check the result:**```bash
git log --oneline -5  # ver o commit criado
git diff main...HEAD  # ver as mudanças
pytest                # confirmar que os testes passam
```### What to observe and learn

- How Claude decides which files are relevant to the issue
- How it handles ambiguity when the issue is unclear
- How it structures commit messages
- How it handles test failures

### Checkpoint
- [ ] Skill /fix-issue executed successfully
- [ ] Branch created with correct nomenclature
- [ ] Commit made with reference to the issue
- [ ] Tests passing after fix

---

## Step 6 — Create your own custom skill

**Estimated time: 45 minutes**

### Objectives
- Understand the anatomy of a skill
- Create a skill from scratch for a flow you use frequently
- Test and refine the skill

### Skill ideas to create

Choose one that makes sense for your project:

-`/deploy`— automate deployment for staging
-`/changelog`— generate CHANGELOG.md from commits
-`/migrate`— create and apply database migrations
-`/seed`— populate the database with test data
-`/perf-test`— run performance tests and generate reports
-`/doc-api`— generate endpoint documentation from code
-`/security-scan`— check for vulnerabilities in the code

### Instructions

**6.1. Create the skill structure:**```bash
mkdir -p .claude/skills/minha-skill
touch .claude/skills/minha-skill/SKILL.md
```**6.2. Minimum structure of SKILL.md:**```markdown
# Skill: minha-skill

## Descrição
[O que a skill faz em 2-3 frases claras]

## Como Usar
`/minha-skill <argumento-opcional>`

## Passos de Execução

### Passo 1 — [Nome do passo]
[O que o Claude deve fazer neste passo]

### Passo 2 — [Nome do passo]
[O que o Claude deve fazer neste passo]

## Ferramentas Disponíveis
| Ferramenta | Uso |
|------------|-----|
| Bash       | [para que usar] |
| Read       | [para que usar] |

## Saída Esperada
[O que o Claude deve apresentar ao final]
```**6.3. Tips for writing a good skill:**

- **Be prescriptive:** use imperative language. "Claude MUST do X before Y"
- **Define the sequence:** number the steps and be explicit about the order
- **Anticipate errors:** include a "Behavior in Special Cases" section
- **Define output:** specify exactly what the user should see at the end
- **Test iteratively:** execute the skill, observe where it deviates from expectations, refine

**6.4. Test the skill:**```
/minha-skill
```Observe the behavior, identify where Claude deviated from the instructions, refine SKILL.md and test again.

**6.5. Skill versioning:**```bash
git add .claude/skills/minha-skill/SKILL.md
git commit -m "Adiciona skill /minha-skill para [descricao]"
```### Checkpoint
- [ ] SKILL.md created with complete structure
- [ ] Skill performed at least once
- [ ] At least one refinement iteration done
- [ ] Skill committed to the repository

---

## Step 7 — Configure MCP servers

**Estimated time: 1 hour**

### Objectives
- Understand the Model Context Protocol (MCP)
- Configure at least 2 MCP servers
- Use MCP tools within Claude Code

### What is MCP

The Model Context Protocol (MCP) is a standardized protocol that allows Claude to connect to external tools and data sources. With MCP servers, Claude can:

- Read and write files (filesystem)
- Interact with GitHub directly
- Consult databases
- Search the web
- Access APIs specific to your domain

### Instructions

**7.1. Understand the structure of mcp-config.json:**

See the example file:```bash
cat exemplos/mcp-config.json
```**7.2. Configure the filesystem MCP server:**

The simplest to start with — allows Claude to work with files more directly:```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/caminho/do/seu/projeto"
      ]
    }
  }
}
```Save to`.claude/mcp-config.json`and start with:```bash
claude --mcp-config .claude/mcp-config.json
```**7.3. Configure the GitHub MCP server:**

Requires a GitHub Personal Access Token:```bash
# Gerar token em: https://github.com/settings/tokens
# Permissões necessárias: repo, read:user
export GITHUB_TOKEN="ghp_..."
```Add to mcp-config.json (see`exemplos/mcp-config.json`for full format).

**7.4. Test MCP servers:**

In Claude Code with MCP enabled:```
Liste os 5 arquivos mais recentemente modificados neste projeto
```

```
Busque as últimas 3 issues abertas no repositório atual
```**7.5. Explore other available MCP servers:**

-`@modelcontextprotocol/server-postgres`— access to PostgreSQL database
-`@modelcontextprotocol/server-sqlite`— access to SQLite database
-`@modelcontextprotocol/server-brave-search`— web search via Brave
-`@modelcontextprotocol/server-puppeteer`— browser automation

Install and test at least one more server in addition to the filesystem.

### Checkpoint
- [ ] mcp-config.json configured with at least 2 servers
- [ ] MCP filesystem working
- [ ] MCP GitHub working
- [ ] Claude can use MCP tools in a real task

---

## Step 8 — Challenge: create a multi-agent system with sub-agents

**Estimated time: 3 hours**

### Objectives
- Understand the concept of agent orchestration
- Create an orchestrator agent that delegates tasks to subagents
- Implement a complete and useful agentic workflow

### The concept of multi-agent

In Claude Code, you can create workflows where:
- An **orchestrator agent** receives a large task and divides it
- **Subagents** execute specific parts in parallel or sequentially
- The orchestrator consolidates the results

### Challenge proposed

Create an **automated code review** system that works like this:

1. Receives a PR or a set of modified files
2. The orchestrator divides the work into 3 parallel analyses:
   - Security Sub-Agent: checks for vulnerabilities
   - Quality Subagent: checks complexity, duplication, readability
   - Testing Sub-Agent: checks test coverage and quality
3. Orchestrator consolidates analytics into a single report

### Instructions

**8.1. Create the orchestrator skill:**```bash
mkdir -p .claude/skills/code-audit
```The Orchestrator SKILL.md must define:
- How to receive the list of files to be analyzed
- How to delegate each analysis to a sub-agent (using the tool`Task`or multiple calls)
- How to consolidate results
- The format of the final report

**8.2. Create sub-agents skills:**```bash
mkdir -p .claude/skills/audit-security
mkdir -p .claude/skills/audit-quality
mkdir -p .claude/skills/audit-tests
```Each sub-agent must have a SKILL.md focused on their specific responsibility.

**8.3. Implement and test:**

Start small: have the orchestrator call just one subagent, then add the others.```
/code-audit app/services/task_service.py
```**8.4. Refine the system:**

After the first test:
- Identify where subagents produced inconsistent results
- Refine the prompts in each subagent's SKILL.md
- Add validation in orchestrator
- Test with more complex files

**8.5. Document the system:**

Create one`SKILL.md`in the skill root directory describing:
- The architecture of the multi-agent system
- How subagents communicate
- How to add new subagents
- Known limitations

### Challenge variations

If the main challenge is too easy, try one of these variations:

**Variation A:** Intelligent deployment system that analyzes changes, decides which environment to test first, runs the tests and only promotes to production if everything passes.

**Variation B:** Refactoring agent that analyzes the code, creates a refactoring plan, runs it in stages with tests between each stage and generates the PR automatically.

**Variation C:** Agentic data pipeline that reads data from multiple sources, transforms, validates data quality, and loads it to the destination — with error handling and automatic retry.

### Checkpoint
- [ ] Functional multi-agent system with at least 2 sub-agents
- [ ] Orchestrator consolidating results correctly
- [ ] At least one real task performed with the system
- [ ] System documentation created

---

## Additional resources to keep learning

### Official documentation
- [Claude Code Docs](https://docs.anthropic.com/claude-code) — complete documentation
- [MCP Specification](https://modelcontextprotocol.io) — protocol specification
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook) — practical examples

### Community
- [Anthropic Discord](https://discord.gg/anthropic) — developer community
- [Claude Code GitHub](https://github.com/anthropics/claude-code) — issues and discussions

### Suggested next challenges
1. Integrate Claude Code into your CI/CD pipeline
2. Create a monitoring agent that analyzes logs and automatically creates issues
3. Implement an automatic documentation system that keeps the README up to date
4. Create a database migration agent that checks compatibility before applying