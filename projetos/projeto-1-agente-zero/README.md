# Project 1 — Agent of Zero

![Status](https://img.shields.io/badge/status-stable-brightgreen)
![Python](https://img.shields.io/badge/python-3.11%2B-blue)
![License](https://img.shields.io/badge/licença-MIT-lightgrey)
![Anthropic](https://img.shields.io/badge/Anthropic-API-orange)

> **Python agent without frameworks — pure agentic loop demonstration**

Implementation of an AI agent from scratch using Anthropic's API directly, without any high-level framework like LangChain or LlamaIndex. The objective is to understand the fundamental mechanism behind any agent: the **Think → Act → Observe** loop.

---

## Summary

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [How to use](#how-to-use)
- [Project structure](#project-structure)
- [How to add new tools](#how-to-add-new-tools)
- [Tests](#tests)
- [License](#license)

---

## Prerequisites

- **Python 3.11 or higher** — the project uses typing and syntax features available from this version
- **Anthropic Account** with API access — create your account at [console.anthropic.com](https://console.anthropic.com)
- **Anthropic API Key** (`ANTHROPIC_API_KEY`) — generated in your account dashboard
-`pip`updated (`pip install --upgrade pip`)

---

## Installation

Follow the steps below in the order presented:```bash
# 1. Clone o repositório principal
git clone https://github.com/seu-usuario/agentic-masterclass.git

# 2. Entre na pasta do projeto
cd agentic-masterclass/projetos/projeto-1-agente-zero

# 3. (Recomendado) Crie e ative um ambiente virtual
python -m venv .venv
source .venv/bin/activate       # Linux / macOS
# .venv\Scripts\activate        # Windows

# 4. Instale as dependências
pip install -r requirements.txt

# 5. Copie o arquivo de variáveis de ambiente
cp .env.example .env

# 6. Abra o arquivo .env e preencha sua chave de API
# ANTHROPIC_API_KEY=sk-ant-...
```> **Attention:** never commits the file`.env`with your real key. It is already listed on`.gitignore`.

---

## How to use

### Interactive mode```bash
python agent.py
```The agent will start in interactive conversation mode. Type your message and press Enter:```
Agente do Zero — duclub
Pressione Ctrl+C para sair

Você: Quanto é 15% de 2400?
============================================================
AGENTE INICIADO
============================================================

[Iteração 1] Pensando...
  → Usando ferramenta: calcular({"expressao": "2400 * 0.15"})
  ← Resultado: Resultado: 360...

============================================================
RESPOSTA FINAL:
============================================================
15% de 2400 é igual a 360.
```### Examples of prompts to test

| Category | Example prompt |
|-----------|------------------|
| Mathematics |`"Calcule a raiz quadrada de 144"`|
| Mathematics |`"Qual é o resultado de (500 * 1.1) ** 2?"`|
| Memory |`"Salve uma nota chamada 'reunião' com o conteúdo 'dia 10 às 14h'"`|
| Search |`"Busque informações sobre inteligência artificial generativa"`|
| Combined |`"Calcule 12 * 8 e salve o resultado como nota 'multiplicação'"`|

### Leaving the agent

Enter`sair`, `exit` ou `quit`, or press`Ctrl+C`.

### Using via code```python
from agent import executar_agente

resultado = executar_agente("Quanto é 2 + 2?")
print(resultado)  # "2 + 2 é igual a 4."
```---

## Project structure```
projeto-1-agente-zero/
│
├── agent.py               # Código principal — loop agentic completo
├── requirements.txt       # Dependências Python (anthropic, python-dotenv)
├── .env.example           # Modelo de variáveis de ambiente
├── .env                   # Suas variáveis locais (não commitado)
├── Makefile               # Atalhos de comandos (install, run, test, lint)
│
├── tools/                 # Ferramentas extras (plug-and-play)
│   ├── __init__.py        # Exporta schemas e funções das ferramentas
│   └── weather_tool.py    # Exemplo: consulta de clima (OpenWeatherMap)
│
├── tests/                 # Testes unitários
│   └── test_agent.py      # Testes com unittest e mocks
│
├── README.md              # Este arquivo
├── CONTRIBUTING.md        # Guia de contribuição
└── ROTEIRO.md             # Roteiro de estudo passo a passo
```### Description of each file

- **`agent.py`** — heart of the project. Defines the tools (`tools`), the dispatcher (`executar_ferramenta`) and the agentic loop (`executar_agente`). Read this file to understand everything.
- **`requirements.txt`** — only two dependencies:`anthropic`(official SDK) and`python-dotenv`(load the`.env`).
- **`tools/weather_tool.py`** — sample tool that demonstrates how to extend the agent with new capabilities without changing the`agent.py`.
- **`tests/test_agent.py`** — unit tests that validate the dispatcher and the agentic loop with mocks, without consuming your API quota.

---

## How to add new tools

The agent is designed to be easily extensible. Follow the 4 steps below:

### Step 1 — Create the tool module in`tools/`

```python
# tools/minha_ferramenta.py

def get_minha_ferramenta_schema() -> dict:
    """Retorna o schema JSON que a API Anthropic espera."""
    return {
        "name": "minha_ferramenta",
        "description": "Descreva o que esta ferramenta faz.",
        "input_schema": {
            "type": "object",
            "properties": {
                "parametro": {
                    "type": "string",
                    "description": "Descreva o parâmetro."
                }
            },
            "required": ["parametro"]
        }
    }

def minha_ferramenta(parametro: str) -> str:
    """Implementação da ferramenta."""
    return f"Resultado para: {parametro}"
```### Step 2 — Export to`tools/__init__.py`

```python
from .minha_ferramenta import get_minha_ferramenta_schema, minha_ferramenta
__all__ = [..., "get_minha_ferramenta_schema", "minha_ferramenta"]
```### Step 3 — Register the tool in`agent.py`

```python
from tools import get_minha_ferramenta_schema, minha_ferramenta

tools = [
    # ... ferramentas existentes ...
    get_minha_ferramenta_schema(),  # adicione aqui
]
```### Step 4 — Add the case to the dispatcher```python
def executar_ferramenta(nome: str, inputs: dict) -> str:
    # ... cases existentes ...
    elif nome == "minha_ferramenta":
        return minha_ferramenta(inputs["parametro"])
```Ready. The agent will automatically know how to use the new tool.

---

## Tests```bash
# Rodar todos os testes
python -m unittest tests/test_agent.py -v

# Ou via Makefile
make test
```Tests **do not consume your API quota** — the Anthropic client is mocked with`unittest.mock`.

---

## License

This project is licensed under the **MIT License**.```
MIT License

Copyright (c) 2025 duclub

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
