# Skill: fix-issue

## Description

The skill`fix-issue`Automates the entire process of fixing a GitHub issue. Upon receiving an issue number, Claude searches the repository for details, analyzes the related code, creates a dedicated branch, implements the fix, runs the tests and commits the changes — all autonomously.

This skill is ideal for well-described bugs, code improvements, documentation adjustments and specific refactorings.

---

## How to Use```
/fix-issue <numero-da-issue>
```### Examples```bash
# Corrigir a issue #123
/fix-issue 123

# Corrigir a issue #45 (bug report)
/fix-issue 45

# Corrigir a issue #200 (feature request simples)
/fix-issue 200
```---

## Execution Steps

Claude will **obligatorily** follow this sequence when executing the skill:

### Step 1 — Read the Issue

Fetch full issue details from GitHub using the CLI`gh`:

```bash
gh issue view <numero> --json title,body,labels,assignees,comments
```Analyze:
- Problem title and description
- Labels (bug, enhancement, documentation, etc.)
- Comments with additional information
- Mentioned files or functions

### Step 2 — Analyze Related Code

Identify the files relevant to the issue:

- Search for functions, classes or variables mentioned in the issue
- Read identified files completely before any editing
- Understand the context: how the code is used, what dependencies exist
- Check existing tests for the affected area

Tools used in this step:`Grep`, `Glob`, `Read`### Step 3 — Create Dedicated Branch

Create a branch with standardized naming before any modifications:```bash
# Para bugs
git checkout -b fix/issue-<numero>-descricao-curta

# Para melhorias
git checkout -b feat/issue-<numero>-descricao-curta

# Para documentação
git checkout -b docs/issue-<numero>-descricao-curta
```Never make changes directly to`main` ou `develop`.

### Step 4 — Implement the Fix

Apply the necessary corrections following the project conventions defined in the`CLAUDE.md`:

- Edit only the necessary files
- Maintain existing code style
- Add type hints where they are missing
- Update docstrings if function signature changed
- Do not introduce new dependencies unnecessarily

Tools used in this step:`Edit`, `Write`### Step 5 — Run the Tests

Check that the fix didn't break anything and, ideally, add tests for the corrected case:```bash
# Rodar todos os testes
pytest

# Rodar apenas os testes relacionados ao fix
pytest tests/test_<modulo>.py -v

# Verificar cobertura na área modificada
pytest --cov=app/<modulo> --cov-report=term-missing
```If any test fails:
1. Analyze the error
2. Fix the problem
3. Run the tests again
4. Repeat until everyone passes

### Step 6 — Commit the Changes

After all tests pass, commit with a clear message and reference to the issue:```bash
# Adicionar arquivos modificados
git add <arquivos-modificados>

# Commitar com referência à issue
git commit -m "Fix: corrige <descricao-do-problema>

Resolve #<numero-da-issue>

- <detalhe 1 do que foi feito>
- <detalhe 2 do que foi feito>"
```Do not use`git add .`— add only the actually modified files to the fix.

---

## Available Tools

| Tool | Use in Skill |
|------------|----------------------------------------------------------|
|`Bash`| To execute`gh issue view`, `git`, `pytest`, `ruff`         |
| `Read`| Read code files before editing |
|`Edit`| Apply corrections to identified files |
|`Write`| Create new test files when necessary |
|`Grep`| Search for functions, classes and patterns in the code |
|`Glob`| Find files related to issue |

---

## Behavior in Special Cases

### Issue too broad or ambiguous

If the issue does not have enough detail to implement a clear solution, Claude should:
1. List what was understood
2. Present possible approaches
3. Ask the user which path to follow before modifying any file

### Tests failing before fix

If the tests are already failing before the modification, Claude should:
1. Inform the user about pre-existing failed tests
2. Ask whether to fix just the issue or also the broken tests
3. Document the tests that were failed at commit

### Fix requires changes to multiple modules

If the fix impacts more than 3 different files, Claude must:
1. Present a detailed plan before executing
2. Wait for user confirmation
3. Execute changes incrementally, testing at each stage

---

## Expected Output

At the end, Claude must present a summary:```
Fix concluído para a issue #<numero>

Branch criada: fix/issue-<numero>-<descricao>
Arquivos modificados:
  - app/routers/tasks.py (linha 45: corrigido filtro de data)
  - tests/test_tasks.py (adicionado teste para o caso de data inválida)

Testes: 42 passed, 0 failed
Commit: "Fix: corrige filtro de data inválida no endpoint de tarefas"

Próximos passos:
  git push origin fix/issue-<numero>-<descricao>
  gh pr create --title "Fix #<numero>: <descricao>"
```
