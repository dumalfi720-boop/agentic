# ROADMAP — Mastering the OpenAI Codex CLI

> Step-by-step roadmap to go from scratch to using Codex CLI in production pipelines.
> Estimated total time: **~6 hours**

---

## Overview of the itinerary```
Passo 1: Instalar (10 min)
    ↓
Passo 2: Obter API key (5 min)
    ↓
Passo 3: Criar AGENTS.md (30 min)
    ↓
Passo 4: Configurar .codex/config.toml (20 min)
    ↓
Passo 5: Primeira sessao interativa (30 min)
    ↓
Passo 6: Modo auto-edit (30 min)
    ↓
Passo 7: Integrar com GitHub Actions (1h)
    ↓
Passo 8: Pipeline de desenvolvimento autonomo (3h)
```---

## Step 1 — Install Node.js and Codex CLI

**Estimated time:** 10 minutes

### 1.1 Check if Node.js is installed```bash
node --version
```If it appears`v18.0.0`or higher, skip to item 1.3.

### 1.2 Install Node.js (if necessary)

**Linux (Ubuntu/Debian):**```bash
# Usando o gerenciador de versoes nvm (recomendado)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
nvm install 20
nvm use 20
```**macOS:**```bash
# Com Homebrew
brew install node@20
```**Windows:**
Go to [nodejs.org](https://nodejs.org) and download the LTS installer.

### 1.3 Install Codex CLI```bash
npm install -g @openai/codex
```### 1.4 Verify the installation```bash
codex --version
# Deve exibir a versao instalada, ex: 0.1.x
```**Checkpoint:** The command`codex --version`it should work without errors.

---

## Step 2 — Obtain the OpenAI API key and configure

**Estimated time:** 5 minutes

### 2.1 Get the key

1. Access [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
2. Click **"Create new secret key"**
3. Copy the key (starts with`sk-...`) — it will not be displayed again

### 2.2 Configure in the environment

**Recommended option — permanently add to shell:**```bash
# Para bash
echo 'export OPENAI_API_KEY="sk-SUA_CHAVE_AQUI"' >> ~/.bashrc
source ~/.bashrc

# Para zsh
echo 'export OPENAI_API_KEY="sk-SUA_CHAVE_AQUI"' >> ~/.zshrc
source ~/.zshrc
```**Alternative option — .env file in project:**```bash
cp .env.example .env
# Edite o arquivo .env e adicione sua chave
```### 2.3 Check the configuration```bash
echo $OPENAI_API_KEY
# Deve exibir sua chave (ou parte dela)
```### 2.4 Test the connection```bash
codex "diga 'funcionou' em uma palavra"
# O agente deve responder brevemente
```**Checkpoint:** Codex must respond without authentication errors.

---

## Step 3 — Create your AGENTS.md

**Estimated time:** 30 minutes

O`AGENTS.md`It is the heart of its agentic configuration. It is automatically read by Codex at the beginning of each session, giving context about the project without you having to repeat it every time.

### 3.1 Understand the structure of AGENTS.md

Open the reference template:```bash
cat AGENTS.md
```The essential sections are:
1. **Project Context** — what the project does, technology stack
2. **Development Commands** — how to install, run, test
3. **Code Conventions** — nomenclature, imports, error pattern
4. **Rules of Behavior** — what the agent should NEVER do
5. **Architecture** — folder structure, architectural patterns

### 3.2 Create for your project```bash
# No diretorio do SEU projeto (nao neste diretório)
cd /caminho/do/seu/projeto

# Copiar o template
cp /home/nmaldaner/projetos/agentic/projetos/projeto-4-codex/AGENTS.md ./AGENTS.md

# Abrir no editor e personalizar
code AGENTS.md   # VS Code
# ou
nano AGENTS.md   # terminal
```### 3.3 What to customize

Replace each section with actual information from your project:

- [ ] Project name and description
- [ ] Technological stack (language, framework, database)
- [ ] Installation and execution commands
- [ ] Test and lint commands
- [ ] Your team naming conventions
- [ ] List of critical files that the agent should not modify
- [ ] Project folder structure

### 3.4 Tips for a good AGENTS.md

- **Be specific:** "Use camelCase for variables" is better than "follow good practices"
- **List what not to do:** Explicit prohibitions prevent accidents
- **Keep updated:** Each time you change the stack, update AGENTS.md
- **Include code examples:** The agent learns from concrete examples

**Checkpoint:** You have a`AGENTS.md`customized in your project directory.

---

## Step 4 — Configure .codex/config.toml with suggest mode

**Estimated time:** 20 minutes

The file`.codex/config.toml`controls the default behavior of the Codex. Configure with mode`suggest`guarantees that the agent will never do anything without your approval — perfect for starters.

### 4.1 Create the folder and file```bash
# No diretorio do SEU projeto
mkdir -p .codex

# Copiar a configuracao base
cp /home/nmaldaner/projetos/agentic/projetos/projeto-4-codex/.codex/config.toml ./.codex/config.toml
```### 4.2 Check the configuration```bash
cat .codex/config.toml
```Confirm that`approval_mode = "suggest"`is defined. This means:
- The agent can read all project files
- The agent shows what it would do, but does not execute anything
- You see the suggestions and decide how to proceed

### 4.3 Available configuration options```toml
# Modo de aprovacao (suggest | auto-edit | full-auto)
approval_mode = "suggest"

# Modelo a usar
model = "gpt-4o"

# Temperatura (0.0 = deterministico, 1.0 = criativo)
temperature = 0.2

# Timeout por operacao (segundos)
timeout = 120

# Nivel de log (debug | info | warn | error)
log_level = "info"
```### 4.4 Why start with suggest

In mode`suggest`:
- Zero risk of accidental changes
- You learn what the agent would do before trusting him
- Great for exploring behavior in new projects

**Checkpoint:** The file`.codex/config.toml`exists with`approval_mode = "suggest"`.

---

## Step 5 — First interactive session with the Codex

**Estimated time:** 30 minutes

It's time to talk to the real agent.

### 5.1 Start Codex in the project directory```bash
# No diretorio do seu projeto (que tem AGENTS.md)
cd /caminho/do/seu/projeto
codex
```The Codex goes:
1. Read the`AGENTS.md`automatically
2. Open the interactive prompt
3. Wait for your instructions

### 5.2 Exercises for the first session

Perform these exercises in order. They range from the simplest to the most complex:

**Exercise A — Explore the project (5 min):**```
Voce: "Liste os arquivos mais importantes deste projeto e explique a funcao de cada um"
```**Exercise B — Understanding the code (10 min):**```
Voce: "Explique como funciona o fluxo de autenticacao neste projeto"
```**Exercise C — Ask for a suggestion for improvement (10 min):**```
Voce: "Quais melhorias de seguranca voce sugere para este projeto?"
```**Exercise D — Code suggestion (5 min):**```
Voce: "Mostre como eu adicionaria um endpoint GET /api/users/:id/tasks que retorna as tarefas de um usuario especifico"
```### 5.3 What to observe during the session

- The Codex will reference information from the`AGENTS.md`in the answers
- In mode`suggest`, it shows code diffs but does not apply
- You can ask follow-up questions in the same session
- Use Ctrl+C to exit

### 5.4 Go out and reflect

When finished, answer yourself:
- Did the agent understand the project stack?
- Did the suggestions make sense for the context?
- What is missing from AGENTS.md?

**Checkpoint:** You have completed an interactive session and have ideas on how to improve AGENTS.md.

---

## Step 6 — Auto-edit mode: let the agent edit with approval

**Estimated time:** 30 minutes

Now we will give the agent more autonomy, but still with control over the execution of commands.

### 6.1 Switch to auto-edit mode

Edit the`.codex/config.toml`:
```toml
approval_mode = "auto-edit"
```Or use the flag directly in the command:```bash
codex --approval-mode auto-edit "sua instrucao aqui"
```### 6.2 What changes in auto-edit mode

Allowed without approval:
- Create new files
- Edit existing files
- Read any project file

Still requires approval:
- Run commands in the terminal (`npm test`, `git commit`, etc.)
- Install dependencies
- Any network operation

### 6.3 Exercises for auto-edit mode

**Exercise A — Create a new file (10 min):**```bash
codex --approval-mode auto-edit "crie um arquivo src/utils/format-date.js com uma funcao formatDate que recebe uma data ISO e retorna no formato DD/MM/AAAA"
```Review the created file and verify that:
- [ ] The file was created in the correct location
- [ ] The code follows the project conventions (according to AGENTS.md)
- [ ] The function does what was requested

**Exercise B — Refactor existing code (20 min):**```bash
codex --approval-mode auto-edit "no arquivo [escolha um arquivo existente], refatore para melhorar a legibilidade sem mudar o comportamento"
```Use`git diff`to see exactly what changed:```bash
git diff
```### 6.4 Recommended workflow```bash
# 1. Criar branch antes de deixar o agente editar
git checkout -b ai/refactor-user-module

# 2. Rodar o Codex em modo auto-edit
codex --approval-mode auto-edit "sua tarefa"

# 3. Revisar todas as mudancas
git diff

# 4. Rodar os testes
npm test

# 5. Se tudo OK, commitar
git add .
git commit -m "refactor: [descricao do que o agente fez]"
```**Checkpoint:** Did you use auto-edit mode and review the changes with`git diff`.

---

## Step 7 — Integrate with GitHub Actions (headless CI/CD)

**Estimated time:** 1 hour

Codex in headless mode (without human interaction) is perfect for CI/CD pipelines.

### 7.1 Understanding headless mode

In headless mode, the Codex:
- Receives the instruction as a command line argument
- Runs without interactive prompt
- Returns the result on stdout
- Returns exit code 0 (success) or 1 (failure)```bash
# Exemplo de uso headless
codex --approval-mode full-auto --quiet "rodar os testes e reportar falhas"
echo "Codigo de saida: $?"
```### 7.2 Copy the example workflow```bash
# No repositorio do seu projeto
mkdir -p .github/workflows
cp /home/nmaldaner/projetos/agentic/projetos/projeto-4-codex/exemplos/github-actions.yml .github/workflows/ai-review.yml
```### 7.3 Configure the API key secret

On GitHub:
1. Go to **Settings > Secrets and variables > Actions**
2. Click on **"New repository secret"**
3. Name:`OPENAI_API_KEY`4. Value: Your API Key

### 7.4 Adapt the workflow to your project

Abra`.github/workflows/ai-review.yml`and customize:```yaml
# Altere a instrucao para o review do seu projeto
run: |
  codex --approval-mode suggest \
    "Revise este PR buscando:
    1. Bugs e erros logicos
    2. Problemas de seguranca
    3. Violacoes das convencoes do AGENTS.md
    4. Oportunidades de melhoria de performance"
```### 7.5 Test the workflow```bash
# Criar um PR de teste
git checkout -b test/codex-integration
echo "// teste" >> src/index.js
git add .
git commit -m "test: testar integracao com Codex"
git push origin test/codex-integration
# Abrir PR no GitHub e observar o workflow rodar
```### 7.6 Advanced uses of CI/CD

**Automatic PR review:**```yaml
run: codex --approval-mode suggest "revise este PR para bugs e melhorias"
```**Automatic changelog generation:**```yaml
run: codex --approval-mode auto-edit "gere um CHANGELOG.md baseado nos commits desde a ultima tag"
```**Test coverage check:**```yaml
run: codex --approval-mode full-auto "identifique funcoes sem testes e crie testes unitarios basicos"
```**Checkpoint:** You have a working GitHub Actions workflow that uses Codex.

---

## Step 8 — Final challenge: autonomous development pipeline

**Estimated time:** 3 hours

This is the final step — and the most advanced. You will build a pipeline where Codex runs a complete development cycle with minimal human intervention.

### 8.1 The challenge

Implement a complete feature using the following pipeline:```
Requisito em linguagem natural
    ↓ [Codex — modo suggest]
Plano de implementacao aprovado
    ↓ [Codex — modo auto-edit]
Codigo escrito e refatorado
    ↓ [Codex — modo full-auto]
Testes gerados e executados
    ↓ [Review humano]
PR criado e aprovado
```### 8.2 Feature to implement

Choose one of these features for your example project:

**Option A (easy):** Search endpoint with filters```
"Implemente um endpoint GET /api/tasks/search que aceita
os parametros: q (texto), status, priority, dueDate.
Inclua validacao de input, paginacao e testes."
```**Option B (medium):** Notification system```
"Crie um sistema de notificacoes em tempo real usando
Server-Sent Events (SSE) para notificar o usuario quando
uma tarefa e atribuida a ele."
```**Option C (advanced):** Smart cache```
"Implemente uma camada de cache Redis para os endpoints
mais acessados. O cache deve ser invalidado automaticamente
quando os dados relevantes sao modificados."
```### 8.3 Run the pipeline

**Phase 1 — Planning (20 min):**```bash
codex --approval-mode suggest "Quero implementar [FEATURE ESCOLHIDA].
Crie um plano detalhado de implementacao com:
- Arquivos a criar/modificar
- Ordem de implementacao
- Pontos de atencao e riscos
- Estimativa de linhas de codigo"
```Review the plan and decide if you agree. Adjust if necessary.

**Phase 2 — Implementation (1h):**```bash
# Criar branch
git checkout -b feature/[nome-da-feature]

# Deixar o agente implementar
codex --approval-mode auto-edit "Implemente [FEATURE] conforme o plano aprovado.
Siga todas as convencoes do AGENTS.md."
```Review each change with`git diff`before continuing.

**Phase 3 — Tests (40 min):**```bash
codex --approval-mode full-auto "
1. Rode os testes existentes e confirme que nao quebramos nada
2. Gere testes unitarios para o codigo novo
3. Gere testes de integracao para os endpoints novos
4. Verifique se a cobertura atingiu 80%"
```**Phase 4 — Documentation (20 min):**```bash
codex --approval-mode auto-edit "
Atualize a documentacao:
1. Adicione os novos endpoints ao Swagger (se existir)
2. Atualize o README com as novas funcionalidades
3. Adicione comentarios JSDoc nas funcoes publicas novas"
```**Phase 5 — Review and PR (40 min):**```bash
# Revisar tudo
git diff main

# Rodar testes finais
npm test
npm run lint

# Criar PR
git push origin feature/[nome-da-feature]
# Abrir PR no GitHub com descricao do que foi implementado
```### 8.4 Post-challenge reflection

When finished, document your learnings:

- Where was the agent most helpful?
- Where did the agent make a mistake and need human correction?
- Does AGENTS.md need updates?
- What would be the next step to automate more?

### 8.5 Going further

After completing the challenge, try:```bash
# Multi-agente: um agente planeja, outro implementa
codex "crie um plano detalhado para refatorar o modulo X" > plano.md
codex --approval-mode auto-edit "implemente o plano em plano.md"

# Revisao de seguranca automatica
codex --approval-mode suggest "faca uma auditoria de seguranca completa e liste vulnerabilidades"

# Atualizacao de dependencias segura
codex --approval-mode full-auto "atualize as dependencias desatualizadas, rodando os testes apos cada atualizacao"
```---

## Summary and next steps

Congratulations! By completing this itinerary, you learned to:

- [x] Install and configure Codex CLI
- [x] Create an effective AGENTS.md to give context to the agent
- [x] Use the 3 approval modes strategically
- [x] Integrate Codex into CI/CD pipelines with GitHub Actions
- [x] Execute a complete agentic development cycle

**Recommended upcoming projects:**
- Project 5: Multi-agent with CrewAI
- Project 6: Agent with customized tools (MCP)
- Project 7: Monitoring and observability of agents

---

*Part of the agentic development course — Project 4 of 8*