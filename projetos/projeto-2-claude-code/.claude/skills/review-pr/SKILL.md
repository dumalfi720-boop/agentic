# Skill: review-pr

## Description

The skill`review-pr`performs a structured and complete review of a GitHub Pull Request. Upon receiving the PR number, Claude searches for changes, analyzes code quality, checks test coverage, checks project conventions, and produces an organized, actionable review comment — ready to be posted or used as a reference.

This skill is ideal for ensuring consistency in reviews, speeding up the code review process and maintaining code quality as a team.

---

## How to Use```
/review-pr <numero-do-pr>
```### Examples```bash
# Fazer review do PR #78
/review-pr 78

# Fazer review do PR #12
/review-pr 12

# Fazer review do PR #200 antes de aprovar
/review-pr 200
```---

## Execution Steps

Claude will **obligatorily** follow this sequence when executing the skill:

### Step 1 — Search for PR information

Collect all relevant Pull Request metadata:```bash
gh pr view <numero> --json title,body,author,baseRefName,headRefName,additions,deletions,changedFiles,reviews,comments,labels,isDraft,mergeable
```Analyze:
- Title and description: does the PR clearly explain what it does and why?
- Author and context
- Origin and destination branch
- Size: number of files and lines changed
- Status: draft, mergeable, or blocked by checks
- Existing labels and reviews

If the PR is as draft (`isDraft: true`), inform the user before continuing — draft PRs are generally not ready for review.

### Step 2 — Get the changes in detail

Fetch the full PR diff:```bash
gh pr diff <numero>
```Analyze line by line:
- Which files were added, modified or removed
- If the changes are cohesive (a PR must do just one thing)
- If there is commented code or forgotten debugs (console.log, print, pdb, etc.)
- Whether there are hardcoded secrets, URLs or environment settings

Tools used in this step:`Bash`### Step 3 — Analyze the modified files

For each relevant file modified, read the full context (not just the diff):

Tools used in this step:`Read`, `Glob`, `Grep`For each file analyzed, check:

**Code quality:**
- Is the code readable and self-explanatory?
- Are there very long functions or methods that should be broken?
- Is there code duplication that could be extracted?
- Is error handling adequate?
- Are there untreated edge cases?

**Project conventions (according to CLAUDE.md):**
- Is the naming of variables, functions and classes correct?
- Type hints present where mandatory?
- Docstrings following the standard format?
- Imports organized in the correct order?
- Code style consistent with the rest of the project?

**Security:**
- Are user inputs being validated?
- Are there operations that should have authorization verification?
- Sensitive data being logged or exposed?

### Step 4 — Check test coverage

Identify whether modified files have matching tests:```bash
# Listar arquivos de teste existentes
gh pr diff <numero> --name-only
```Tools used in this step:`Glob`, `Grep`, `Read`For each modified code file:
- Check if a corresponding test file exists
- Read existing tests: do they cover PR cases?
- Has the PR added new tests for new features?
- Are there tests for the identified edge cases?```bash
# Verificar se os testes passam localmente (se possível)
pytest tests/ -v --tb=short 2>&1 | tail -20
```### Step 5 — Check PR checks

View the status of automated checks:```bash
gh pr checks <numero>
```Identify:
- Which checks are failing
- Which checks are passing
- Are there lint, type or security checks failing?

### Step 6 — Produce the structured review

Generate a complete review comment, organized into the following sections:```
## Review do PR #<numero>: <titulo>

### Resumo
[2-3 frases sobre o objetivo do PR e impressão geral]

### Pontos Positivos
- [O que está bem feito — seja específico]
- [Boa prática adotada]

### Problemas Críticos (bloqueiam merge)
- **[arquivo:linha]** — [descrição do problema] | Sugestão: [como corrigir]

### Melhorias Sugeridas (não bloqueiam)
- **[arquivo:linha]** — [sugestão de melhoria]
- [Consideração de design]

### Testes
- [ ] [Caso de teste faltando] em `tests/test_<modulo>.py`
- [x] [Caso coberto adequadamente]

### Checklist Final
- [ ] Testes passando
- [ ] Lint sem erros
- [ ] Sem código de debug
- [ ] Documentação atualizada (se necessário)
- [ ] Variáveis de ambiente documentadas (se necessário)

### Decisão
**[APROVADO / APROVADO COM RESSALVAS / MUDANÇAS NECESSÁRIAS]**

[Justificativa da decisão em 1-2 frases]
```---

## Available Tools

| Tool | Use in Skill |
|------------|-----------------------------------------------------------------------|
|`Bash`| To execute`gh pr view`, `gh pr diff`, `gh pr checks`, `pytest`      |
| `Read`| Read modified files in full context |
|`Grep`| Search for problematic patterns (debug, secrets, TODOs) |
|`Glob`| Find test files corresponding to changed files |

---

## Behavior in Special Cases

### Very large PR (>500 lines changed)

If the diff is very large:
1. Inform the user of the size of the PR
2. Focus on the highest risk files (business logic, authentication, data)
3. Mention in the review that the PR could be broken into smaller parts

### PR without description

If the PR has no description or the description is very vague:
1. Include in the review that the lack of context makes the review difficult
2. Suggest what the description should include
3. Continue the review anyway

### Checks failing

If automated checks are failing:
1. List the checks that are failing in the review
2. Classify as critical issue if lint, types or tests
3. Not approving the PR while critical checks are failing

### PR belongs to the user

If the PR was created by the user who invoked the skill:
1. Warn about conflict of interest in self-review
2. Continue the review objectively as a pair review
3. Focus on quality improvements, not approval

---

## Expected Output

When finished, Claude must present the formatted review and ask:```
Review concluido para PR #<numero>: "<titulo>"

[Review completo formatado acima]

---
Deseja que eu:
  1. Poste este review como comentario no PR: gh pr review <numero> --comment -b "..."
  2. Aprove o PR: gh pr review <numero> --approve
  3. Solicite mudancas: gh pr review <numero> --request-changes -b "..."
  4. Apenas usar como referencia (nao postar)
```Wait for the user's decision before taking any action on GitHub.