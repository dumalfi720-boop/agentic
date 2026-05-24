# Contribution Guide

Thank you for your interest in contributing to **Project 1 — Agent of Zero**! This guide describes the process for reporting issues, proposing improvements, and adding new features.

---

## Summary

- [How to fork and open a PR](#how-to-fork-and-open-a-pr)
- [How to add a new tool](#how-to-add-a-new-tool)
- [Commit messages pattern](#commit-messages-pattern)
- [How to run the tests](#how-to-run-the-tests)
- [General good practices](#general-good-practices)

---

## How to fork and open a PR

### 1. Fork the repository

Click the **Fork** button on GitHub to create a copy of the repository in your account.

### 2. Clone your fork locally```bash
git clone https://github.com/SEU-USUARIO/agentic-masterclass.git
cd agentic-masterclass/projetos/projeto-1-agente-zero
```### 3. Configure the remote upstream (original repository)```bash
git remote add upstream https://github.com/DUVIP-CLUB/agentic-masterclass.git
```### 4. Keep your fork updated before you start```bash
git fetch upstream
git checkout main
git merge upstream/main
```### 5. Create a branch with a descriptive name

Use the pattern`tipo/descricao-curta`:

```bash
git checkout -b feat/ferramenta-traducao
# ou
git checkout -b fix/calcular-divisao-por-zero
# ou
git checkout -b docs/melhorar-readme
```### 6. Make your changes

- Follow the code standards outlined in this guide
- Add tests for any new functionality
- Check the syntax before committing:```bash
  make lint
  ```### 7. Commit following the message pattern

See the [Commit messages pattern](#commit-messages-pattern) section below.

### 8. Upload the branch to your fork```bash
git push origin feat/ferramenta-traducao
```### 9. Open the Pull Request

On GitHub, click **Compare & pull request**. Fill in:
- Clear and objective **Title** (follow the commit pattern)
- **Description** explaining what was done, why and how to test
- Check if the PR resolves any issue with`Closes #123`---

## How to add a new tool

Adding a tool to the agent follows a structured process. Use the template below as a starting point.

### Tool template

Create a file in`tools/nome_da_ferramenta.py`:

```python
"""
tools/nome_da_ferramenta.py
Breve descrição do que esta ferramenta faz.

Dependências externas (se houver):
    pip install nome-do-pacote

Configuração (se houver):
    Adicione ao .env:
    NOME_DA_FERRAMENTA_API_KEY=sua_chave
"""

import os


def get_nome_da_ferramenta_schema() -> dict:
    """
    Retorna o schema JSON da ferramenta no formato Anthropic.

    Returns:
        dict: Schema completo com name, description e input_schema.
    """
    return {
        "name": "nome_da_ferramenta",
        "description": "Descreva claramente o que a ferramenta faz e quando usá-la.",
        "input_schema": {
            "type": "object",
            "properties": {
                "parametro_obrigatorio": {
                    "type": "string",
                    "description": "Descreva o que este parâmetro representa."
                },
                "parametro_opcional": {
                    "type": "integer",
                    "description": "Parâmetro opcional com valor padrão."
                }
            },
            "required": ["parametro_obrigatorio"]
        }
    }


def nome_da_ferramenta(parametro_obrigatorio: str, parametro_opcional: int = 10) -> str:
    """
    Implementa a lógica da ferramenta.

    Args:
        parametro_obrigatorio (str): Descrição do parâmetro.
        parametro_opcional (int): Descrição. Padrão: 10.

    Returns:
        str: Resultado formatado como string legível.
    """
    # TODO: implementar a lógica real aqui
    return f"Resultado para '{parametro_obrigatorio}' com opcao={parametro_opcional}"
```### Integration step by step

**1. Register the export in`tools/__init__.py`:**

```python
from .weather_tool import get_weather_tool_schema, get_weather
from .nome_da_ferramenta import get_nome_da_ferramenta_schema, nome_da_ferramenta  # adicione

__all__ = [
    "get_weather_tool_schema", "get_weather",
    "get_nome_da_ferramenta_schema", "nome_da_ferramenta",  # adicione
]
```**2. Import and register the schema in`agent.py`:**

```python
from tools import get_weather_tool_schema, get_weather
from tools import get_nome_da_ferramenta_schema, nome_da_ferramenta  # adicione

tools = [
    # ... ferramentas existentes ...
    get_nome_da_ferramenta_schema(),  # adicione
]
```**3. Add the case to the dispatcher`agent.py`:**

```python
def executar_ferramenta(nome: str, inputs: dict) -> str:
    # ... cases existentes ...

    elif nome == "nome_da_ferramenta":
        return nome_da_ferramenta(
            inputs["parametro_obrigatorio"],
            inputs.get("parametro_opcional", 10)
        )

    return f"Ferramenta '{nome}' não encontrada."
```**4. Add tests in`tests/test_agent.py`:**

```python
def test_dispatch_nome_da_ferramenta(self):
    """Ferramenta 'nome_da_ferramenta': deve retornar resultado esperado."""
    resultado = agent.executar_ferramenta(
        "nome_da_ferramenta",
        {"parametro_obrigatorio": "valor de teste"}
    )
    self.assertIn("valor de teste", resultado)
```**5. Document in README.md** by adding the tool to the file tree and extension section.

---

## Commit message pattern

We follow the [Conventional Commits](https://www.conventionalcommits.org/) standard. Each message must have the format:```
tipo(escopo): descrição curta em minúsculo
```### Allowed types

| Type | When to use |
|------|-------------|
|`feat`| New functionality or tool |
|`fix`| Bug Fix |
|`docs`| Changes to documentation only |
|`test`| Addition or correction of tests |
|`refactor`| Refactoring without changing behavior |
|`chore`| Maintenance tasks (deps, CI, etc.) |
|`style`| Code formatting without logical change |

### Examples```bash
# Boa mensagem — clara, no padrão
git commit -m "feat(tools): adicionar ferramenta de consulta de CEP"
git commit -m "fix(dispatcher): tratar divisao por zero em calcular"
git commit -m "docs(readme): atualizar secao de instalacao com venv"
git commit -m "test(agent): adicionar teste para max_iteracoes"
git commit -m "refactor(loop): extrair logica de impressao para funcao separada"

# Mensagens que devem ser evitadas
git commit -m "fix"          # muito vago
git commit -m "wip"          # nunca commite WIP na main
git commit -m "Alterações"   # sem contexto
```### Commits with breaking changes

If your change breaks backwards compatibility, add`!`after the type and explain in the body of the message:```bash
git commit -m "feat(agent)!: renomear executar_ferramenta para dispatch_tool

BREAKING CHANGE: a funcao executar_ferramenta foi renomeada para dispatch_tool.
Atualize todas as importacoes diretas."
```---

## How to run the tests

### Run all tests```bash
python -m unittest tests/test_agent.py -v
```### Run a specific test```bash
python -m unittest tests.test_agent.TestDispatcher.test_dispatch_calcular -v
```### Run all tests in a class```bash
python -m unittest tests.test_agent.TestDispatcher -v
```### Via Makefile```bash
make test
```### Check syntax```bash
make lint
```### What to expect

All tests must pass without the need for an API key configured:```
test_dispatch_buscar_na_web ... ok
test_dispatch_calcular ... ok
test_dispatch_calcular_erro_sintaxe ... ok
test_dispatch_calcular_expressao_complexa ... ok
test_dispatch_ferramenta_invalida ... ok
test_dispatch_ferramenta_invalida_nome_no_retorno ... ok
test_dispatch_salvar_multiplas_notas ... ok
test_dispatch_salvar_nota ... ok
test_dispatch_salvar_nota_persistencia ... ok
test_agente_max_iteracoes ... ok
test_agente_resposta_final ... ok
test_agente_retorna_string ... ok
test_agente_usa_ferramenta_e_responde ... ok

----------------------------------------------------------------------
Ran 13 tests in 0.003s

OK
```---

## General good practices

- **Does not commit the file`.env`** — it contains secrets and is already in`.gitignore`.
- **Write docstrings** for all public functions. Follow the Google Style format.
- **Comments in English** — Python standard, facilitates international contributions.
- **Text for the user in Portuguese** — messages displayed in the terminal and high-level docstrings.
- **One tool per file** — keep each tool in its own module in`tools/`.
- **Do not break the public interface** —`executar_agente(mensagem, max_iteracoes)`is the stable interface. When refactoring, keep the signature compatible.
- **Prefer`str`as feedback from tools** — the agent inserts the feedback directly into the context of the conversation; always return readable strings.