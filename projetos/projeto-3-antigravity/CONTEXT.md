# CONTEXT.md — Antigravity / Gemini Agent Context

> This file is automatically loaded by Antigravity as persistent context for the Gemini agent.
> Equivalent to Claude Code's CLAUDE.md, but optimized for the Gemini model and the Antigravity environment.

---

## Project Identity

**Name:** my-agentic-project
**Version:** 1.0.0
**Description:** Web application developed with agentic-first methodology using Antigravity IDE.
**Repository:** https://github.com/usuario/meu-projeto-agentic
**Environment:** Antigravity (Google Agent-First IDE)
**Primary model:** gemini-2.0-flash-exp

---

## Technology Stack

### Frontend
- **Framework:** React 18 with TypeScript
- **Styling:** Tailwind CSS 3.x
- **Build tool:** Vite 5.x
- **State management:** Zustand
- **Routing:** React Router v6

###Backend
- **Runtime:** Node.js 20 LTS
- **Framework:** Fastify 4.x
- **ORM:** Prisma with PostgreSQL
- **Authentication:** Google OAuth 2.0 via Firebase Auth
- **Cache:** Redis 7.x

### Infrastructure
- **Cloud:** Google Cloud Platform
- **Backend hosting:** Cloud Run (serverless containers)
- **Frontend hosting:** Firebase Hosting
- **Database:** Cloud SQL (PostgreSQL 15)
- **CI/CD:** GitHub Actions + Cloud Build

### Development Tools
- **IDE:** Antigravity (agent-first)
- **Version control:** Git + GitHub
- **Tests:** Vitest (unit), Playwright (E2E)
- **Linting:** ESLint + Prettier
- **Monitoring:** Google Cloud Monitoring + Sentry

---

## Directory Structure```
meu-projeto-agentic/
├── CONTEXT.md              # Este arquivo — contexto do agente
├── ARCHITECTURE.md         # Documentação de arquitetura
├── workspace.json          # Configuração do Antigravity
├── src/
│   ├── frontend/           # Aplicação React
│   │   ├── components/     # Componentes reutilizáveis
│   │   ├── pages/          # Páginas da aplicação
│   │   ├── hooks/          # React hooks customizados
│   │   ├── stores/         # Estado global (Zustand)
│   │   └── utils/          # Funções utilitárias
│   └── backend/            # API Fastify
│       ├── routes/         # Definição de rotas
│       ├── services/       # Lógica de negócio
│       ├── middleware/      # Middlewares
│       └── prisma/         # Schema e migrações
├── tests/
│   ├── unit/               # Testes unitários (Vitest)
│   └── e2e/                # Testes E2E (Playwright)
├── infra/
│   ├── terraform/          # Infraestrutura como código
│   └── docker/             # Dockerfiles
└── .github/
    └── workflows/          # CI/CD pipelines
```---

## Code Conventions

### Nomenclature
- **React component files:** PascalCase —`UserProfile.tsx`- **Hooks files:** camelCase with prefix`use` — `useAuthState.ts`- **Backend service files:** camelCase —`userService.ts`- **Constants:** UPPER_SNAKE_CASE —`MAX_RETRY_ATTEMPTS`- **Variables and functions:** camelCase —`fetchUserData()`- **TypeScript types and interfaces:** PascalCase —`UserProfileData`### TypeScript
- Always use explicit typing in public functions
- Avoid`any`- to use`unknown`when the type is not known
- Prefer`interface`for objects,`type`for unions and intersections
- Export types along with implementations in the same file

###React
- Functional components with hooks (without class components)
- Props always typed with dedicated interface
- Avoid prop drilling — use Zustand for shared state
- Memorization only when necessary (not prematurely)

### Commits
- Follow Conventional Commits:`feat:`, `fix:`, `docs:`, `refactor:`, `test:`- Messages in English, present imperative: "Add user authentication"
- Maximum 72 characters in the title
- Commit body explains the "why", not the "what"

### Tests
- Minimum coverage: 80% for critical business logic
- Each service function must have at least one unit test
- E2E testing for critical user flows (login, checkout, etc.)

---

## Automated Workflows

### Workflow: New Feature (`feature`)
The agent follows this loop when receiving a request for new functionality:

1. **Plan** — Analyzes the order, maps affected files, describes necessary changes
2. **Implement** — Write code following the conventions defined above
3. **Test** — Create unit tests and check if they pass
4. **Review** — Reviews generated code before committing, waits for human approval

**Trigger:**`@agent implement feature: [descrição]`### Workflow: Bug Fix (`bugfix`)
1. **Analyze** — Reproduce the bug, identify the root cause
2. **Fix** — Implements the minimum necessary fix
3. **Verify** — Runs relevant tests and confirms that the bug is fixed

**Trigger:**`@agent fix bug: [descrição do problema]`### Workflow: Refactoring (`refactor`)
1. **Audit** — Identifies code smells and opportunities for improvement
2. **Refactor** — Applies improvements without changing external behavior
3. **Validate** — Ensures all tests pass after refactoring

**Trigger:**`@agent refactor: [arquivo ou componente]`### Workflow: Deploy (`deploy`)
1. **Build** — Compiles frontend and backend
2. **Test** — Runs complete suite of tests
3. **Push** — Pushes to the container registry
4. **Deploy** — Deploy to Cloud Run / Firebase Hosting
5. **Verify** — Verifies health check on critical endpoints

**Trigger:**`@agent deploy to [staging|production]`---

## Domain Context

### Main Entities
- **User** — User authenticated via Google OAuth
- **Project** — Project created by the user
- **Task** — Task within a project
- **Comment** — Comment on a task

### Critical Business Rules
1. A user can only see projects they are a member of
2. Only the project owner can delete the project
3. Archived tasks do not appear in the default list
4. Deleted comments show "[Comment removed]" to preserve threading
5. Limit of 100 active projects per user on the free plan

### Glossary
- **Workspace:** Set of projects within an organization
- **Sprint:** Work period (standard 2 weeks)
- **Velocity:** Number of tasks completed per sprint
- **Burndown:** Sprint progress chart

---

## External Integrations

### Google Workspace
- **Google Calendar:** Synchronization of task deadlines
- **Google Drive:** File attachments in tasks
- **Gmail:** Email notifications

### Authentication Configuration```typescript
// src/backend/config/auth.ts
export const authConfig = {
  provider: 'google',
  clientId: process.env.GOOGLE_CLIENT_ID,
  scopes: ['email', 'profile', 'openid'],
  callbackUrl: `${process.env.APP_URL}/auth/callback`
}
```---

## Environment Variables

### Mandatory (development)```bash
DATABASE_URL="postgresql://user:pass@localhost:5432/myapp"
REDIS_URL="redis://localhost:6379"
GOOGLE_CLIENT_ID="seu-client-id.apps.googleusercontent.com"
GOOGLE_CLIENT_SECRET="seu-client-secret"
JWT_SECRET="chave-secreta-minimo-32-chars"
```### Mandatory (production)```bash
# Injete via Google Secret Manager — nunca hardcode
DATABASE_URL         # Via Cloud SQL connector
REDIS_URL            # Via Memorystore
GOOGLE_CLIENT_ID     # Via Secret Manager
GOOGLE_CLIENT_SECRET # Via Secret Manager
JWT_SECRET           # Via Secret Manager
SENTRY_DSN           # Via Secret Manager
```---

## Restrictions and Policies

### What the Agent CAN do automatically
- Create and modify source code files
- Run build and test commands
- Create branches and commits (never stops`main`directly)
- Install npm dependencies (only from trusted sources)
- Make read calls to the GitHub API

### What the Agent MUST ask for approval before
- Modify infrastructure configuration files (`.tf`, `cloudbuild.yaml`)
- Push code to any remote branch
- Perform database migrations
- Modify environment variables or secrets
- Deploy to any environment

### What the Agent should NEVER do
- Commit directly to the branch`main` ou `production`- Store or log credentials, tokens or passwords
- Install dependencies with known vulnerabilities (CVSS > 7.0)
- Make network calls to domains not listed in approved integrations
- Delete production data

---

## Useful Commands```bash
# Desenvolvimento local
npm run dev              # Inicia frontend + backend em modo dev
npm run test             # Executa todos os testes
npm run test:e2e         # Executa testes E2E com Playwright
npm run lint             # Verifica linting
npm run build            # Build de produção

# Banco de dados
npx prisma migrate dev   # Aplica migrações em desenvolvimento
npx prisma studio        # Interface visual do banco de dados
npx prisma generate      # Regenera o Prisma Client

# Docker
docker-compose up -d     # Sobe serviços locais (postgres, redis)
docker-compose down      # Para os serviços

# Deploy
gcloud run deploy        # Deploy manual no Cloud Run
firebase deploy          # Deploy manual no Firebase Hosting
```---

## Notes for Agent Gemini

- Always read this file before doing any task
- When creating new files, follow the naming conventions defined above
- Before implementing, check if there is similar code that can be reused
- Prefer simple and idiomatic solutions over unnecessary abstractions
- When in doubt about a design decision, ask before implementing
- Document non-obvious architectural decisions with in-code comments
- Keep this file updated when the stack or conventions change

---

*Last updated: 2026-03-03*
*Maintained by: Development Team + Gemini Agent*