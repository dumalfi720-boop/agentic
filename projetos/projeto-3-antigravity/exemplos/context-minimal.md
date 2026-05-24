# CONTEXT.md — Minimum Version

> Reduced version of CONTEXT.md for simple projects or for those just starting out.
> Copy the content below to a file`CONTEXT.md`at the root of your project
> and replace the values in square brackets.

---

## How to use this template

This is the minimum set of information that the Gemini agent needs to be useful in your project. For more complex projects, use the`CONTEXT.md`full version available in the root of this repository.

**Rule of thumb:** If you spend more than 30 minutes writing CONTEXT.md, it's too detailed to begin with. Start with the minimum, add details as the agent asks questions or makes mistakes.

---

## Minimum template (copy from here)```markdown
# CONTEXT.md

> Contexto persistente do projeto para o agente Gemini no Antigravity.

## Projeto

**Nome:** [nome-do-projeto]
**Descricao:** [Uma frase descrevendo o que o projeto faz]
**Repositorio:** [https://github.com/usuario/repositorio]

## Stack

[Liste apenas as tecnologias que voce realmente usa]

- **Linguagem:** [ex: TypeScript, Python, Go]
- **Frontend:** [ex: React 18, Vue 3, ou "nao tem frontend"]
- **Backend:** [ex: Fastify, FastAPI, ou "nao tem backend"]
- **Banco de dados:** [ex: PostgreSQL, MongoDB, ou "nao tem banco"]
- **Hospedagem:** [ex: Vercel, Heroku, Cloud Run]

## Convencoes

- **Linguagem dos commits:** [portugues / ingles]
- **Formato de commits:** [ex: Conventional Commits — feat:, fix:, docs:]
- **Estilo de codigo:** [ex: "seguir o .eslintrc existente" ou "indentacao com 2 espacos"]
- **Onde ficam os testes:** [ex: tests/, __tests__/, ou "sem testes ainda"]

## Restricoes importantes

### O agente PODE fazer automaticamente:
- Criar e modificar arquivos de codigo
- Executar comandos de build e teste
- Instalar dependencias npm/pip/go

### O agente DEVE pedir aprovacao antes de:
- Fazer qualquer push para o repositorio remoto
- Modificar arquivos de configuracao de deploy
- Executar comandos que afetam o banco de dados

### O agente NUNCA deve:
- Fazer commit diretamente para a branch main/master
- Armazenar senhas ou tokens no codigo
- Deletar arquivos sem confirmacao explica

## Comandos principais

```bash
# Install dependencies
[ex: npm install]

# Run in development
[ex: npm run dev]

# Run tests
[ex: npm test]

# Production build
[ex: npm run build]```

---
*Ultima atualizacao: [data]*
```---

## Filling guide

### "Project" — what to write

The description should answer: "What does this software do for the user?"

Good examples:
- "Appointment scheduling system for medical clinics"
- "API to process payments via PIX"
- "Dashboard to view sales metrics in real time"

Bad (very vague) examples:
- "Web system"
- "REST API"
- "College Project"

### "Stack" — what to include

Only include what is relevant to the agent when writing code. If you don't have a database, don't write "no database". Just omit that line.

Tip: Think about what a new developer would need to know to open a Pull Request on your project.

### "Conventions" — what is most important

The commit convention is the most important for the agent. If your commits are in English, declare that. The agent will generate commits in the language and format you specify.

If you have a file`.eslintrc` ou `prettier.config.js`, you can simply write "follow ESLint/Prettier configuration" and the agent will read these files automatically.

### "Restrictions" — do not skip this section

The restrictions are the most critical part. An unrestricted agent can push to main, run migrations in production or delete files without asking. Always define what requires human approval.

For simple personal projects, a minimum set of restrictions is:```markdown
### O agente PODE fazer automaticamente:
- Criar e modificar arquivos de codigo local
- Executar testes

### O agente DEVE pedir aprovacao antes de:
- Qualquer operacao git (commit, push, branch)
- Qualquer instalacao de dependencia nova

### O agente NUNCA deve:
- Modificar arquivos fora da pasta do projeto
```---

## When expanding CONTEXT.md

Expand to the full CONTEXT.md when:

- The agent will start generating code inconsistent with the project style
- You find yourself repeating the same instructions in every conversation
- The project will grow and have business rules that the agent needs to know
- You add external integrations that the agent will need to use
- The team grows and needs documented conventions anyway

---

## Filled example — simple frontend project```markdown
# CONTEXT.md

> Contexto persistente do projeto para o agente Gemini no Antigravity.

## Projeto

**Nome:** portfolio-pessoal
**Descricao:** Site de portfolio pessoal com projetos, blog e formulario de contato
**Repositorio:** https://github.com/joao/portfolio

## Stack

- **Linguagem:** TypeScript
- **Frontend:** Astro 4 com componentes React para partes interativas
- **Banco de dados:** nao tem — conteudo estatico via Markdown
- **Hospedagem:** Vercel (deploy automatico via GitHub)

## Convencoes

- **Linguagem dos commits:** portugues
- **Formato de commits:** "Adiciona X", "Corrige Y", "Atualiza Z" (sem prefixo)
- **Estilo de codigo:** seguir o .eslintrc e prettier.config.js existentes
- **Onde ficam os testes:** sem testes automatizados por ora

## Restricoes importantes

### O agente PODE fazer automaticamente:
- Criar e modificar arquivos de codigo e conteudo
- Executar npm run build para verificar se compila

### O agente DEVE pedir aprovacao antes de:
- Qualquer operacao git
- Adicionar novas dependencias ao package.json
- Modificar arquivos de configuracao (astro.config.mjs, vercel.json)

### O agente NUNCA deve:
- Commitar ou fazer push sem aprovacao explicita
- Remover posts do blog existentes

## Comandos principais

```bash
npm install # Install dependencies
npm run dev # Development server (localhost:4321)
npm run build # Production build
npm run preview # Production build preview```

---
*Ultima atualizacao: 2026-03-03*
```---

*For the complete CONTEXT.md with all sections, see`/CONTEXT.md`at the root of this project.*