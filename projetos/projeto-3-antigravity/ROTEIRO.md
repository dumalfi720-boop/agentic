# SCRIPT — Mastering Antigravity

Step-by-step guide to configuring and using Antigravity in a real agentic development project.

**Estimated total time:** ~5h30min (in a single day or divided into sessions)

---

## Overview of the itinerary```
Passo 1 → Acesso e instalacao          (15 min)
Passo 2 → Workspace e configuracao     (10 min)
Passo 3 → CONTEXT.md personalizado     (30 min)
Passo 4 → Servidores MCP               (20 min)
Passo 5 → Primeiro workflow            (30 min)
Passo 6 → Integracao Google Cloud      (1h)
Passo 7 → Desafio Plan→Execute→Verify  (3h)
```---

## Step 1 — Request beta access and install Antigravity

**Estimated time: 15 minutes**

### What is this step

Antigravity is still in closed beta. You will need to request access before you begin. While waiting for approval (it can take hours to days), prepare your environment.

### How to request access

1. Visit [g.co/antigravity](https://g.co/antigravity) with your Google account
2. Click on **"Request Access"** and fill out the form:
   - Briefly describe how you intend to use Antigravity
   - Mention previous experience with Claude Code, GitHub Copilot or similar tools — this increases the chances of quick approval
3. You will receive a confirmation email in your Gmail when access is granted

### Prepare the environment while waiting

Even without Antigravity, install the necessary dependencies:```bash
# Verificar versao do Node.js (precisa ser 20+)
node --version

# Se precisar instalar ou atualizar o Node.js:
# Acesse https://nodejs.org e baixe a versao LTS

# Verificar git
git --version

# Instalar os servidores MCP (pode fazer agora)
npm install -g @modelcontextprotocol/server-filesystem
npm install -g @modelcontextprotocol/server-github
```### Install Antigravity (after receiving access)

1. Open the approval email and click on the download link
2. Download the installer for your operating system (Linux, macOS or Windows)
3. Run the installer and follow the on-screen instructions
4. On first run, authenticate with your Google account when prompted

### Expected result

- Requested (or granted) access to Antigravity beta
- Node.js 20+ installed
- Git configured
- Antigravity installed and authenticated with Google account

---

## Step 2 — Create workspace and import workspace.json

**Estimated time: 10 minutes**

### What is this step

O`workspace.json`It's the configuration file that tells Antigravity which model to use, which tools to activate, and which workflows are available. You will import it and create your first configured workspace.

### Create a new project

1. Open Antigravity
2. Click **File > New Project** (or use`Ctrl+Shift+N`)
3. Choose a directory for the project — it can be an existing folder with code or a new folder
4. Antigravity will open the empty workspace

### Import workspace.json

**Option A — Import through the interface:**
1. Click **File > Import Workspace Configuration**
2. Browse to the file`workspace.json`of this project and select it
3. Antigravity will load all settings automatically

**Option B — Copy directly:**
1. Copy the file`workspace.json`to the root of your project
2. Antigravity automatically detects and loads when opening the folder

### Check the loaded configuration

In the Antigravity settings panel (gear icon), you should see:

- **Model:**`gemini-2.0-flash-exp`- **Temperature:**`0.2`- **Active tools:** Filesystem MCP, GitHub MCP, Browser, Terminal
- **Available workflows:**`feature`, `bugfix`- **Permissions:** file_write: ask, network: ask, shell_execute: ask

### Initial customizations (optional)

Edit the`workspace.json`to adjust to your project:```json
{
  "name": "nome-do-seu-projeto",    // Altere para o nome real
  "agent": {
    "temperature": 0.1,             // Mais baixo = mais deterministico
    "max_iterations": 10            // Reduza para projetos simples
  }
}
```### Expected result

- Workspace created in Antigravity
-`workspace.json`successfully imported
- Settings visible in the Antigravity panel

---

## Step 3 — Configure your CONTEXT.md

**Estimated time: 30 minutes**

### What is this step

O`CONTEXT.md`and the most important file for agentic development. It gives the Gemini agent the context needed to make the right decisions about YOUR specific project. Without a good CONTEXT.md, the agent will generate generic code that may not fit into your project.

### Use the project template

Copy the`CONTEXT.md`from this project to the root of your project:```bash
cp /caminho/para/projeto-3-antigravity/CONTEXT.md /caminho/para/seu-projeto/CONTEXT.md
```### Edit section by section

Open the`CONTEXT.md`in the editor and customize each section:

#### Section 1 — Project Identity (5 min)```markdown
## Identidade do Projeto

**Nome:** nome-real-do-seu-projeto
**Versao:** 0.1.0
**Descricao:** O que seu projeto faz em uma ou duas frases
**Repositorio:** https://github.com/seu-usuario/seu-repositorio
**Ambiente:** Antigravity (Google Agent-First IDE)
**Modelo primario:** gemini-2.0-flash-exp
```#### Section 2 — Technological Stack (10 min)

Only list what you actually use. Remove everything that does not apply to your project. If you use Vue instead of React, replace it. If you don't have a backend, remove this entire subsection.

Example for a simple frontend project:```markdown
### Frontend
- **Framework:** Vue 3 com TypeScript
- **Estilizacao:** CSS Modules
- **Build tool:** Vite 5.x

### Infraestrutura
- **Hospedagem:** Vercel
```#### Section 3 — Code Conventions (5 min)

Adjust the conventions to what your team actually uses. If you use commits in Portuguese, change it. If you prefer`type`instead of`interface`, document this. The agent will follow exactly what is written here.

#### Section 4 — Business Rules (10 min)

This is the most critical section and the most specific to your project. Document the rules that the agent needs to know in order not to break the system:```markdown
## Regras de Negocio Criticas
1. [Regra mais importante do seu sistema]
2. [Segunda regra mais importante]
3. [...]
```If you don't know which rules to document, think about the questions a new developer would ask in their first week on the job.

#### Section 5 — Restrictions and Policies

Carefully review what the agent can do automatically. To start, keep the configuration conservative: the agent needs to ask permission for almost everything. You can loosen restrictions as you gain confidence.

### Check if CONTEXT.md was loaded

After saving the`CONTEXT.md`, start a conversation with the agent at Antigravity and ask:```
Qual e a stack tecnologica do nosso projeto?
```If the agent responds correctly, the context has been loaded. If he doesn't know, check if the file is in the root of the project and if the`workspace.json`list`CONTEXT.md` em `context_files`.

### Expected result

-`CONTEXT.md`customized for your project
- Agent capable of answering questions about the project stack and conventions

---

## Step 4 — Connect MCP Servers

**Estimated time: 20 minutes**

### What is this step

MCP (Model Context Protocol) servers are processes that run locally and give the agent access to external tools. You will install and configure the two MCP servers used in this project: filesystem and GitHub.

### Install the servers

Run the installation script included in this project:```bash
chmod +x scripts/setup-mcp.sh
./scripts/setup-mcp.sh
```Or install manually:```bash
npm install -g @modelcontextprotocol/server-filesystem
npm install -g @modelcontextprotocol/server-github
```### Configure GitHub server

GitHub's MCP server needs a personal access token:

1. Go to [github.com/settings/tokens](https://github.com/settings/tokens)
2. Click on **"Generate new token (classic)"**
3. Select permissions:
   -`repo`— full access to private repositories
   -`read:user`— read profile information
4. Generate the token and copy it (it will only be shown once)
5. Create a file`.env`in the root of your project:```bash
GITHUB_TOKEN=ghp_seu_token_aqui
```**Important:** add`.env`to your`.gitignore`to not commit the token:```bash
echo ".env" >> .gitignore
```### Check the MCP connection on Antigravity

1. In Antigravity, go to **Settings > MCP Servers**
2. You shall see`filesystem` e `github`listed
3. Click **"Test Connection"** on each one to check

If any server appears as disconnected:
- Check that the`npm install -g`was executed successfully
- Check if the`GITHUB_TOKEN`is defined in`.env`- Restart Antigravity

### Test MCP servers

Start a conversation with the agent and test:```
Liste os 5 arquivos mais recentemente modificados neste projeto.
```The agent must use the filesystem server to respond. If it works, the filesystem MCP is operational.```
Qual e o numero de issues abertas no repositorio deste projeto?
```If the agent can fetch this information, GitHub's MCP is working.

### Expected result

- MCP filesystem and github servers installed
- GitHub token configured on`.env`- Both servers connected and running on Antigravity

---

## Step 5 — Run first automated workflow

**Estimated time: 30 minutes**

### What is this step

You will use the workflow`feature`to implement a simple functionality from scratch. The objective is to understand the complete cycle: prompt → plan → approval → execution → result.

### Choose a simple feature

For this first workflow, choose something small and well-defined. Suggestions:

- Add a health check endpoint (`GET /health`)
- Create a reusable button component
- Add email validation to an existing form
- Create a utility function with tests

### Start the workflow

In the Antigravity chat panel, type:```
@agent implement feature: adicionar endpoint GET /api/health que retorna
{"status": "ok", "version": "1.0.0", "timestamp": "<ISO timestamp>"}
```### Monitor the agent cycle

The agent will execute the workflow in 4 steps:

**Step 1 — Plan:**
The agent will list the files it intends to create or modify. Read carefully. Ask if anything seems strange before approving.

Example of expected plan:```
Plano para implementar GET /api/health:

1. Criar src/backend/routes/health.ts
   - Registrar rota GET /health no Fastify
   - Retornar status, version e timestamp

2. Registrar a rota em src/backend/app.ts
   - Importar e registrar o modulo health

3. Criar tests/unit/health.test.ts
   - Teste: resposta tem status 200
   - Teste: resposta contem campos obrigatorios
   - Teste: timestamp e uma ISO string valida

Nenhuma alteracao em banco de dados ou infraestrutura.
Aprovacao necessaria? (s/n)
```Click **Approve** or write`s`to continue.

**Step 2 — Implement:**
The agent writes the code. You will see files being created in real time. Do not interrupt — wait for the step to finish.

**Step 3 — Test:**
The agent will run the created tests. Follow the exit from the terminal. If a test fails, the agent will automatically attempt to correct it (until`max_iterations`attempts).

**Step 4 — Review:**
The agent presents a summary of what was done and awaits your final approval to commit.

### Evaluate the result

Review the generated code:
- Does it follow the conventions defined in CONTEXT.md?
- Do the tests make sense?
- Is there anything you would change?

If you want adjustments, write in chat:```
O endpoint precisa incluir tambem o campo "environment" com o valor de NODE_ENV.
Faca essa alteracao.
```### Expected result

- Feature implemented by the agent
- Tests created and passing
- Code reviewed and approved
- First complete workflow successfully executed

---

## Step 6 — Integrate with Google Cloud

**Estimated time: 1 hour**

### What is this step

You will connect the project to Google Cloud so that the agent can make automated deployments to Cloud Run and Firebase Hosting. This step requires a Google Cloud account with active billing.

### Prerequisites```bash
# Instalar a Google Cloud CLI
curl https://sdk.cloud.google.com | bash
exec -l $SHELL

# Autenticar
gcloud auth login

# Configurar o projeto
gcloud config set project SEU_PROJETO_ID
```### Configure Firebase```bash
# Instalar Firebase CLI
npm install -g firebase-tools

# Fazer login
firebase login

# Inicializar no projeto
firebase init
# Selecione: Hosting, Functions (se necessario)
# Escolha o projeto criado no passo anterior
```### Configure Cloud Run

1. Enable the Cloud Run API in the Google Cloud Console:
   - Access [console.cloud.google.com](https://console.cloud.google.com)
   - Navigate to **APIs & Services > Library**
   - Search for "Cloud Run" and click **Enable**

2. Configure deployment permissions:```bash
# Criar conta de servico para deploy
gcloud iam service-accounts create antigravity-deploy \
  --display-name="Antigravity Deploy"

# Conceder permissoes necessarias
gcloud projects add-iam-policy-binding SEU_PROJETO_ID \
  --member="serviceAccount:antigravity-deploy@SEU_PROJETO_ID.iam.gserviceaccount.com" \
  --role="roles/run.developer"

# Gerar chave da conta de servico
gcloud iam service-accounts keys create chave-deploy.json \
  --iam-account=antigravity-deploy@SEU_PROJETO_ID.iam.gserviceaccount.com
```3. Add the key to`.env`:

```bash
GOOGLE_APPLICATION_CREDENTIALS=./chave-deploy.json
GOOGLE_CLOUD_PROJECT=SEU_PROJETO_ID
```### Test the deployment workflow```
@agent deploy to staging
```The agent must:
1. Build the application
2. Run the tests
3. Build the Docker image
4. Push to Artifact Registry
5. Deploy to Cloud Run
6. Check the health check of the deployed service

Follow each step and approve those that require confirmation.

### Expected result

- Google Cloud CLI configured and authenticated
- Firebase CLI configured
- Cloud Run enabled and with correct permissions
- First deployment successfully carried out by the agent

---

## Step 7 — Challenge: Complete Plan→Execute→Verify pipeline

**Estimated time: 3 hours**

### What is this step

This is the final challenge — implementing a complete feature using the full agentic cycle, with human review at each critical phase. The goal is to practice effective collaboration with the agent: neither blindly delegating everything, nor micromanaging every line of code.

### The challenge feature

Implement a complete authentication system with the following features:

- Endpoint`POST /api/auth/login`with Google OAuth
- Endpoint`POST /api/auth/logout`- Endpoint`GET /api/auth/me`(returns authenticated user)
- Authentication middleware for protected routes
- Unit tests for each endpoint
- Integration tests for the complete flow
- Documentation of endpoints

### Phase 1 — Plan (30 min)

Start with a detailed prompt:```
@agent implement feature: sistema de autenticacao com Google OAuth.

Requisitos:
- POST /api/auth/login: recebe o Google ID Token no body, valida com o Google,
  cria ou atualiza o usuario no banco, retorna um JWT proprio com 24h de expiracao
- POST /api/auth/logout: invalida o token JWT do usuario (blacklist em Redis)
- GET /api/auth/me: retorna os dados do usuario autenticado (requer JWT valido)
- Middleware authRequired: pode ser aplicado em qualquer rota para exigir autenticacao

Restricoes:
- Nunca armazenar o Google token no banco
- JWT deve conter apenas userId e email (sem dados sensiveis)
- Erros de autenticacao retornam 401, nao 403
```**Your task at this stage:** Review the generated plan. Verify that the agent:
- Mapped all files that need to be created/modified
- Identified the necessary dependencies (e.g. JWT validation library)
- Planned the tests correctly
- Didn't plan to change anything that shouldn't

Ask questions and correct the plan before approving. Example:```
No plano, voce nao mencionou como vai armazenar a blacklist de tokens no Redis.
Qual e a estrategia?
```### Phase 2 — Execute (1h30min)

Approve the plan and monitor its execution. During this phase:

- **Intervene when** the agent appears to be heading in the wrong direction
- **Do not intervene** for aesthetic or style adjustments — let the agent finish and make adjustments later
- **Document** decisions that the agent makes and that you find relevant

If the agent gets stuck on an error after multiple attempts:```
O problema que voce esta tentando resolver e [descricao do problema].
Uma abordagem alternativa seria [sua sugestao]. Tente por esse caminho.
```### Phase 3 — Verify (1h)

After the agent finishes the implementation:

1. **Run the tests manually:**```bash
npm run test
npm run test:e2e
```2. **Test endpoints manually with curl or Postman:**```bash
# Obtenha um Google ID Token de teste primeiro
curl -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"idToken": "seu_google_id_token"}'
```3. **Review the code with the agent:**```
Revise a implementacao do middleware authRequired e identifique
possiveis vulnerabilidades de seguranca.
```4. **Deploy for staging:**```
@agent deploy to staging
```5. **Check in staging:**```
@agent verify deployment: teste os endpoints de autenticacao em staging
e confirme que estao funcionando corretamente
```### Completion criteria

The challenge is complete when:

- [ ] All unit tests pass
- [ ] All integration tests pass
- [ ] Endpoints work in a local environment
- [ ] The deployment for staging was carried out successfully
- [ ] Endpoints work in staging
- [ ] Can you explain each decision the agent made

### Post-challenge reflection

After completing, answer these questions to consolidate your learning:

1. When did you intervene and why?
2. Did the agent make any decisions that you would not have made? Was it a better or worse decision?
3. How long do you estimate it would take to implement the same feature manually?
4. What would you change about CONTEXT.md based on what you learned in this challenge?

### Expected result

- Complete authentication system implemented by the agent
- Tests passing (unitary and integration)
- Deployment carried out successfully in staging
- Deep understanding of the agentic cycle and how to effectively collaborate with the agent

---

*Roadmap Conclusion: You now have hands-on experience with the full cycle of agentic development using Antigravity.*

*Last update: 2026-03-03*