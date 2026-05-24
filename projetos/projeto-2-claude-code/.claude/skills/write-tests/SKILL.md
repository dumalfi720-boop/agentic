# Skill: write-tests

## Description

The skill`write-tests`Automatically generates unit tests for an existing code file. Upon receiving the file path, Claude reads the code, identifies all public functions and methods that do not yet have tests, creates a complete test file with adequate coverage, and validates the tests by running pytest — all autonomously.

This skill is ideal for increasing test coverage in legacy code, ensuring that new features are tested from the beginning and maintaining project quality.

---

## How to Use```
/write-tests <caminho/do/arquivo>
```### Examples```bash
# Gerar testes para um service
/write-tests app/services/task_service.py

# Gerar testes para um router
/write-tests app/routers/tasks.py

# Gerar testes para um módulo utilitário
/write-tests app/utils/validators.py
```---

## Execution Steps

Claude will **obligatorily** follow this sequence when executing the skill:

### Step 1 — Read the target file

Read the complete file to understand the entire code:

Tool used:`Read`When reading the file, identify and document:
- All public functions (do not start with`_`)
- All public class methods
- Input parameters: expected types, optional values, defaults
- Return values: type and possible values
- Exceptions that can be raised
- External dependencies: database, APIs, services

### Step 2 — Check existing tests

Before creating any test, check if there are already tests for the file:

Tool used:`Glob`, `Grep`

```python
# Padrões de arquivos de teste a procurar:
# tests/test_<modulo>.py
# tests/<modulo>_test.py
# <mesmo_diretorio>/test_<modulo>.py
```For each function identified in Step 1, check if there is already a corresponding test:```bash
# Buscar por nome da função nos arquivos de teste
grep -r "def test_<nome_da_funcao>" tests/
```Generate the list of:
- Functions already tested (skip or supplement if coverage is poor)
- Functions without any testing (maximum priority)

### Step 3 — Analyze existing fixtures and conftest

Read the`conftest.py`to understand the available fixtures:

Tool used:`Glob`, `Read`

```bash
# Buscar conftest.py nos diretórios de teste
find tests/ -name "conftest.py"
```Identify:
- Database fixtures available (`db`, `session`, `client`)
- Authenticated user fixtures (`auth_headers`, `user`)
- Mocks and patches already configured
- Test environment settings

### Step 4 — Plan the test cases

For each untested function, plan the scenarios:

**For each function, create tests for:**

1. **Happy path** — valid input, expected output
2. **Border Values** — zero, empty, None, empty string, empty list
3. **Invalid entries** — wrong type, invalid format
4. **Expected Exceptions** — verify that the correct exception is raised
5. **Extreme states** — maximum, minimum value, very long list

**Example cases by type of function:**

For create functions:```python
def test_create_task_sucesso()          # dados válidos → task criada
def test_create_task_titulo_vazio()     # título vazio → ValidationError
def test_create_task_usuario_invalido() # user_id inválido → HTTPException 404
```For search functions (get/list):```python
def test_get_task_existente()           # ID válido → task retornada
def test_get_task_nao_encontrada()      # ID inválido → HTTPException 404
def test_list_tasks_pagina_vazia()      # sem tasks → lista vazia
def test_list_tasks_paginacao()         # muitas tasks → respeita limit/offset
```For update functions:```python
def test_update_task_sucesso()          # dados parciais → task atualizada
def test_update_task_sem_permissao()    # outro usuário → HTTPException 403
```### Step 5 — Create the test file

Create the test file following the project conventions:

Tool used:`Write`**Standard test file structure:**```python
"""Testes para <nome_do_modulo>.

Cobre as funções: <lista_de_funcoes>
"""
import pytest
from unittest.mock import MagicMock, patch

# Imports do módulo sendo testado
from app.services.task_service import create_task, get_task, list_tasks


# =============================================================================
# Fixtures específicas para este módulo
# =============================================================================

@pytest.fixture
def task_data_valida():
    """Dados mínimos válidos para criar uma task."""
    return {
        "title": "Tarefa de teste",
        "description": "Descrição da tarefa",
        "priority": "medium"
    }


# =============================================================================
# Testes: create_task
# =============================================================================

class TestCreateTask:
    """Testes para a função create_task."""

    def test_sucesso_dados_validos(self, db, task_data_valida):
        """Deve criar a task quando os dados são válidos."""
        ...

    def test_falha_titulo_vazio(self, db):
        """Deve levantar ValidationError quando o título está vazio."""
        ...


# =============================================================================
# Testes: get_task
# =============================================================================

class TestGetTask:
    """Testes para a função get_task."""
    ...
```**Mandatory conventions:**
- Group tests by function into classes`TestNomeDaFuncao`- Descriptive test names:`test_<cenario>_<resultado_esperado>`- Docstring in each test explaining what is being tested
- Use`pytest.raises()`to test exceptions
- Do not duplicate setup logic — use fixtures

### Step 6 — Validate the tests

After creating the file, run the tests to ensure they pass:

Tool used:`Bash`

```bash
# Rodar apenas os testes recém-criados
pytest tests/test_<modulo>.py -v

# Se falhar, ver o traceback completo
pytest tests/test_<modulo>.py -v --tb=long

# Verificar cobertura do módulo
pytest tests/test_<modulo>.py --cov=app/<modulo> --cov-report=term-missing
```If any test fails:
1. Analyze the error: is it a problem in the test or in the code?
2. If there is a problem with the test: correct the test logic
3. If there is a problem in the code: inform the user before changing the production code
4. Run again until everyone passes
5. If production code has bugs exposed by testing, report without auto-fixing

---

## Available Tools

| Tool | Use in Skill |
|------------|---------------------------------------------------------------------|
|`Read`| Read target file and conftest.py |
|`Write`| Create the generated test file |
|`Bash`| Run pytest to validate, search for files with find |
|`Grep`| Check existing tests, search for imports and fixtures |
|`Glob`| Find test files and conftest.py |
|`Edit`| Adjust tests that need fixing after the first run |

---

## Behavior in Special Cases

### File with many functions (>10 public functions)

If the file has many functions:
1. Inform the user how many functions were found
2. Ask whether to generate tests for all or a subset
3. Prioritize critical business functions over simple utility functions

### Functions with external dependencies that are difficult to mock

If the function accesses the database, external APIs, or file system:
1. Identify dependencies
2. Use appropriate mocks/patches
3. Document in the test comment what is being mocked and why

### Existing tests with low coverage

If a test file already exists but coverage is low:
1. Do not overwrite the existing file
2. Identify missing test cases
3. Create a file`test_<modulo>_adicional.py`with the new tests
4. Or ask the user if they want to merge with the existing file

### Code without type hints

If the file has no type hints:
1. Infer types from code
2. Warn in the final summary that the lack of type hints made it difficult to generate accurate tests
3. Suggest adding type hints to the production file

---

## Expected Output

At the end, Claude must present a summary:```
Testes gerados para: app/services/task_service.py

Arquivo criado: tests/test_task_service.py

Funcoes cobertas:
  - create_task     → 4 testes (happy path, titulo vazio, usuario invalido, duplicado)
  - get_task        → 3 testes (existente, nao encontrada, sem permissao)
  - list_tasks      → 4 testes (vazia, com dados, paginacao, filtro por status)
  - update_task     → 3 testes (sucesso, nao encontrada, sem permissao)
  - delete_task     → 2 testes (sucesso, sem permissao)

Total: 16 testes gerados

Resultado do pytest:
  16 passed em 2.34s

Cobertura de app/services/task_service.py: 94%
Linhas nao cobertas: 45, 67 (tratamento de erro de conexao com o banco)

Funcoes nao cobertas (fora do escopo ou muito simples):
  - _validate_internal  (privada, testada indiretamente)
```
