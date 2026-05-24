# AGENTS.md — Context Guide for Codex CLI

> This file is automatically read by the OpenAI Codex CLI in every session.
> Equivalent to CLAUDE.md in the Anthropic ecosystem.
> Keep this file updated as the project evolves.

---

## 1. Project Context

**Name:** Task Manager API
**Version:** 2.3.1
**Type:** REST API — Web application backend

### What is this project
A RESTful API built with Node.js and Express to manage tasks, projects and users. Used by three different frontends: a SPA in React, a mobile app in React Native and a desktop client in Electron. The API is the source of truth for all data.

### Technology Stack
- **Runtime:** Node.js 20 LTS
- **Framework:** Express 4.18
- **Database:** PostgreSQL 15 (ORM: Prisma 5)
- **Cache:** Redis 7
- **Authentication:** JWT + Refresh Tokens
- **Validation:** Zod
- **Tests:** Jest + Supertest
- **Linting:** ESLint (config: airbnb-base) + Prettier
- **Build:** without build step (native ESM Node)
- **Deploy:** Docker + docker-compose on VPS (production), Railway (staging)

---

## 2. Development Commands

### Installation```bash
# Instalar dependências
npm install

# Configurar banco de dados (primeira vez)
npm run db:migrate
npm run db:seed

# Verificar se tudo está funcionando
npm run health-check
```### Development```bash
# Rodar servidor em modo desenvolvimento (hot reload com nodemon)
npm run dev

# Servidor escuta em: http://localhost:3000
# Documentação Swagger: http://localhost:3000/api-docs
```### Build and Production```bash
# Verificar se o código está pronto para produção
npm run lint
npm run typecheck
npm test

# Rodar em modo produção (não usar em dev)
npm start
```### Database```bash
# Criar nova migration
npm run db:migrate:create -- --name nome-da-migration

# Aplicar migrations pendentes
npm run db:migrate

# Resetar banco (CUIDADO — apaga todos os dados)
npm run db:reset

# Abrir Prisma Studio (interface visual)
npm run db:studio
```### Cache and Utilities```bash
# Limpar cache Redis
npm run cache:flush

# Gerar tipos TypeScript do Prisma
npm run db:generate
```---

## 3. Test Commands```bash
# Rodar todos os testes unitários
npm test

# Rodar testes com watch mode (desenvolvimento)
npm run test:watch

# Rodar testes de integração (requer banco de dados de teste)
npm run test:integration

# Rodar testes E2E (requer servidor rodando)
npm run e2e

# Cobertura de código (meta: mínimo 80%)
npm run test:coverage

# Rodar apenas testes de um arquivo específico
npx jest src/modules/tasks/tasks.service.test.js

# Rodar testes que correspondem a um padrão
npx jest --testNamePattern="deve criar uma tarefa"
```### Test Configuration
- Test database:`task_manager_test`(configured in`.env.test`)
- Unit tests use Prisma Client mocks
- Integration tests use real banking with transactions rolled back after each test
- E2E tests use full server on port 3001

---

## 4. Code Conventions

### Nomenclature
- **Variables and functions:** camelCase (`getUserById`, `taskTitle`)
- **Classes:** PascalCase (`UserService`, `TaskRepository`)
- **Constants:** SCREAMING_SNAKE_CASE (`MAX_RETRY_ATTEMPTS`, `DEFAULT_PAGE_SIZE`)
- **Files:** kebab-case (`user-service.js`, `task-repository.js`)
- **Module folders:** kebab-case (`user-management/`, `task-scheduler/`)
- **Bank tables:** snake_case plural (`users`, `task_assignments`)
- **Bank columns:** snake_case (`created_at`, `user_id`)

### Imports```javascript
// Ordem obrigatória de imports (ESLint enforces)
// 1. Módulos nativos do Node
import { readFile } from 'node:fs/promises';
import path from 'node:path';

// 2. Dependências externas
import express from 'express';
import { z } from 'zod';

// 3. Imports internos (usar alias @/)
import { UserService } from '@/modules/users/user.service.js';
import { AppError } from '@/shared/errors/app-error.js';
import { logger } from '@/shared/logger.js';

// Sempre usar extensão .js nos imports (Node ESM)
// Sempre usar alias @/ para imports internos (configurado em jsconfig.json)
```### ESLint — Important Rules```javascript
// PROIBIDO — sem console.log em produção
console.log('debug'); // Use logger.debug() em vez disso

// PROIBIDO — callbacks aninhados
app.get('/users', (req, res) => {
  db.query('SELECT *', (err, rows) => { // Não faça isso
    res.json(rows);
  });
});

// CORRETO — async/await
app.get('/users', async (req, res, next) => {
  try {
    const users = await userService.findAll();
    res.json(users);
  } catch (error) {
    next(error);
  }
});
```### Error Handling```javascript
// Sempre usar AppError para erros esperados
throw new AppError('Tarefa não encontrada', 404, 'TASK_NOT_FOUND');

// Nunca expor stack traces em produção
// O error handler global em src/shared/middleware/error-handler.js cuida disso

// Sempre logar erros inesperados antes de propagar
logger.error({ error, context: 'UserService.create' }, 'Erro inesperado ao criar usuário');
```### Validation```javascript
// Sempre validar input com Zod nos controllers
const createTaskSchema = z.object({
  title: z.string().min(1).max(200),
  description: z.string().max(2000).optional(),
  dueDate: z.string().datetime().optional(),
  priority: z.enum(['low', 'medium', 'high']).default('medium'),
});

// O middleware validateRequest em src/shared/middleware/validate-request.js
// aplica o schema e retorna 400 automaticamente se inválido
```---

## 5. Behavior Rules for the Agent

### NEVER do this without explicit approval:
- Modify files in`src/migrations/`(migrations are irreversible)
- Change`package.json` ou `package-lock.json`- Modify any file`.env` ou `.env.*`- To execute`npm publish`, `git push --force`, or`docker push`- Modify production configuration files in`config/production/`- Delete database or backup files
- Change Prisma schema definitions without creating corresponding migration

### ALWAYS do:
- Create branches with prefix`feature/`, `fix/`, `chore/`, `refactor/`- Rotate`npm run lint`before any commit
- Rotate`npm test`before proposing a PR
- Add tests for any new code (minimum coverage: 80%)
- Use the logger (`import { logger } from '@/shared/logger.js'`) instead of console
- Check for a broken test before modifying existing code
- Document public functions with JSDoc

### When creating new endpoints:
1. Create route in`src/modules/{modulo}/{modulo}.routes.js`2. Create controller in`src/modules/{modulo}/{modulo}.controller.js`3. Create service in`src/modules/{modulo}/{modulo}.service.js`4. Create repository in`src/modules/{modulo}/{modulo}.repository.js`5. Add Zod validation to the controller
6. Write unit tests for the service
7. Write integration tests for the endpoint
8. Update Swagger documentation on`src/modules/{modulo}/{modulo}.swagger.js`---

## 6. Project Architecture

### Folder Structure```
task-manager-api/
├── src/
│   ├── modules/              # Módulos de domínio (feature folders)
│   │   ├── users/            # Tudo relacionado a usuários
│   │   │   ├── user.routes.js
│   │   │   ├── user.controller.js
│   │   │   ├── user.service.js
│   │   │   ├── user.repository.js
│   │   │   ├── user.schema.js      # Schemas Zod
│   │   │   ├── user.swagger.js     # Documentação OpenAPI
│   │   │   └── user.service.test.js
│   │   ├── tasks/
│   │   ├── projects/
│   │   └── notifications/
│   ├── shared/               # Código compartilhado entre módulos
│   │   ├── middleware/       # Express middlewares
│   │   │   ├── auth.js           # Verificação JWT
│   │   │   ├── error-handler.js  # Handler global de erros
│   │   │   ├── rate-limiter.js   # Rate limiting por IP
│   │   │   └── validate-request.js
│   │   ├── errors/           # Classes de erro customizadas
│   │   │   └── app-error.js
│   │   ├── logger.js         # Pino logger configurado
│   │   └── prisma.js         # Singleton do Prisma Client
│   ├── config/               # Configurações da aplicação
│   │   ├── env.js            # Validação de variáveis de ambiente com Zod
│   │   ├── database.js       # Config do PostgreSQL
│   │   └── redis.js          # Config do Redis
│   └── app.js                # Express app (sem listen — separado do server.js)
├── server.js                 # Entry point — chama app.listen()
├── prisma/
│   ├── schema.prisma         # Schema do banco de dados
│   ├── migrations/           # Migrations geradas pelo Prisma
│   └── seed.js               # Dados iniciais de desenvolvimento
├── tests/
│   ├── integration/          # Testes de integração (Supertest)
│   ├── e2e/                  # Testes E2E
│   └── fixtures/             # Dados de teste reutilizáveis
├── config/
│   ├── development/
│   └── production/           # NUNCA modificar sem aprovação
├── .env.example              # Template de variáveis (sem valores reais)
├── .env                      # Variáveis locais (nunca comitar — no .gitignore)
├── .env.test                 # Variáveis para testes
├── AGENTS.md                 # Este arquivo
├── ARCHITECTURE.md           # Decisões arquiteturais detalhadas
└── README.md                 # Documentação pública
```### Layering Pattern```
Request → Routes → Controller → Service → Repository → Database
                     ↓
                Validação (Zod)
                     ↓
                Middleware (Auth, RateLimit)
```- **Routes:** only definition of routes and applied middleware
- **Controller:** receives request, validates input, calls service, returns response
- **Service:** business logic, orchestrates repositories, does not know HTTP
- **Repository:** access to the bank via Prisma, without business logic

---

## 7. Environment and Environment Variables

### Mandatory Variables (see`.env.example`)
```bash
# Servidor
NODE_ENV=development          # development | staging | production
PORT=3000

# Banco de Dados
DATABASE_URL=postgresql://user:password@localhost:5432/task_manager

# Redis
REDIS_URL=redis://localhost:6379

# Autenticação
JWT_SECRET=seu-secret-aqui-minimo-32-chars
JWT_EXPIRES_IN=15m
REFRESH_TOKEN_SECRET=outro-secret-aqui-minimo-32-chars
REFRESH_TOKEN_EXPIRES_IN=7d

# Email (para notificações)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=noreply@seudominio.com
SMTP_PASS=app-password-aqui

# Monitoramento (opcional em dev)
SENTRY_DSN=https://...@sentry.io/...

# Rate Limiting
RATE_LIMIT_WINDOW_MS=900000   # 15 minutos
RATE_LIMIT_MAX=100            # requisições por janela
```### Environment Variable Rules
- Never hardcode configuration values into code
- Always validate environment variables at startup (see`src/config/env.js`)
- Undefined variables should cause fail fast
- Use`.env.example`as documentation — always keep up to date
- Never commit files`.env`with real values

---

## 8. Main API Endpoints

### Authentication (`/api/auth`)
```
POST   /api/auth/register          # Criar conta
POST   /api/auth/login             # Login, retorna JWT + refresh token
POST   /api/auth/refresh           # Renovar access token
POST   /api/auth/logout            # Invalidar refresh token
POST   /api/auth/forgot-password   # Solicitar reset de senha
POST   /api/auth/reset-password    # Confirmar reset de senha
```### Users (`/api/users`) — requires auth```
GET    /api/users/me               # Perfil do usuário autenticado
PATCH  /api/users/me               # Atualizar perfil
DELETE /api/users/me               # Deletar conta
GET    /api/users/:id              # Perfil público de usuário
```### Tasks (`/api/tasks`) — requires auth```
GET    /api/tasks                  # Listar tarefas (com filtros e paginação)
POST   /api/tasks                  # Criar tarefa
GET    /api/tasks/:id              # Detalhe de uma tarefa
PATCH  /api/tasks/:id              # Atualizar tarefa
DELETE /api/tasks/:id              # Deletar tarefa
POST   /api/tasks/:id/complete     # Marcar como concluída
POST   /api/tasks/:id/assign       # Atribuir a usuário
```### Projects (`/api/projects`) — requires auth```
GET    /api/projects               # Listar projetos do usuário
POST   /api/projects               # Criar projeto
GET    /api/projects/:id           # Detalhe do projeto
PATCH  /api/projects/:id           # Atualizar projeto
DELETE /api/projects/:id           # Arquivar projeto
GET    /api/projects/:id/tasks     # Tarefas do projeto
POST   /api/projects/:id/members   # Adicionar membro
DELETE /api/projects/:id/members/:userId  # Remover membro
```### Health and Monitoring```
GET    /health                     # Health check básico (sem auth)
GET    /health/detailed            # Status detalhado (requer admin)
GET    /api-docs                   # Documentação Swagger UI
```### Common Query Parameters```
?page=1&limit=20          # Paginação
?sort=createdAt:desc      # Ordenação
?filter[status]=pending   # Filtros
?search=texto             # Busca full-text
?include=project,assignee # Relações a incluir
```---

## 9. Response Patterns

### Success```json
{
  "success": true,
  "data": { ... },
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 150,
    "totalPages": 8
  }
}
```### Error```json
{
  "success": false,
  "error": {
    "code": "TASK_NOT_FOUND",
    "message": "Tarefa não encontrada",
    "statusCode": 404
  }
}
```### Validation Error```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Dados inválidos",
    "statusCode": 400,
    "details": [
      { "field": "title", "message": "Título é obrigatório" },
      { "field": "dueDate", "message": "Data inválida" }
    ]
  }
}
```---

## 10. Additional Information for the Agent

### When the agent doesn't know something:
- Consult`ARCHITECTURE.md`for architectural decisions
- Consult the Prisma documentation at`prisma/schema.prisma`- Check existing tests to understand expected behavior
- Don't assume — ask the developer

### Team Context:
- Small team: 3 backend developers, 2 frontend
- Mandatory code review for PRs that touch auth, billing or user data
- Automatic deployment for staging with each merge`develop`- Deployment to production is manual and requires approval from 2 people

### Priorities:
1. Security (never sacrifice for convenience)
2. Reliability (tests before features)
3. Performance (monitor with Sentry and logs)
4. Maintainability (clean and documented code)