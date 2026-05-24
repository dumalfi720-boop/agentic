# Study Guide — Project 1: Agent from Zero

This roadmap guides you from scratch to creating a complete search agent. Follow the steps in order — each step builds on the previous one.

**Estimated total time: ~3h30min**

---

## Step 1 — Understand what an agentic loop is

**Estimated time: 5 minutes of reading**

Before touching code, understand the fundamental concept.

### What is an AI agent?

An agent is a program that uses a language model (LLM) to autonomously make decisions and execute actions, repeating the cycle until it completes a task.

### The agentic loop (Think → Act → Observe)```
┌─────────────────────────────────────────────────────┐
│                   LOOP AGENTIC                      │
│                                                     │
│   Usuário pergunta                                  │
│         ↓                                           │
│   LLM PENSA ──── Tenho informação suficiente? ──── SIM → Responde ao usuário
│         │                                           │
│        NÃO                                          │
│         ↓                                           │
│   LLM AGE ──── Escolhe e chama uma ferramenta       │
│         ↓                                           │
│   OBSERVA ──── Recebe o resultado da ferramenta     │
│         ↓                                           │
│   Volta para PENSA (com o novo contexto)            │
└─────────────────────────────────────────────────────┘
```### Why does this matter?

Without the agentic loop, the LLM can only use what is in its training. With it, the LLM can:
- Make calculations in real time
- Search for updated information
- Save and retrieve data
- Interact with external systems (APIs, databases, files)

### Further reading (optional)

- [Official Anthropic Documentation — Tool Use](https://docs.anthropic.com/en/docs/build-with-claude/tool-use)
- [ReAct: Synergizing Reasoning and Acting in Language Models](https://arxiv.org/abs/2210.03629) (original concept article)

---

## Step 2 — Install dependencies and configure the environment

**Estimated time: 10 minutes**

### 2.1 — Create and activate a virtual environment```bash
cd projetos/projeto-1-agente-zero

# Criar o ambiente virtual
python -m venv .venv

# Ativar (Linux/macOS)
source .venv/bin/activate

# Ativar (Windows)
.venv\Scripts\activate
```You'll know it worked when the terminal prompt shows`(.venv)`at the beginning.

### 2.2 — Install dependencies```bash
pip install -r requirements.txt
```You should install only two packages:`anthropic` e `python-dotenv`.

### 2.3 — Configure the .env file```bash
cp .env.example .env
```Open the file`.env`in any text editor and fill in:```
ANTHROPIC_API_KEY=sk-ant-api03-...
```> How to get your key: go to [console.anthropic.com](https://console.anthropic.com) → API Keys → Create Key.

### 2.4 — Check installation```bash
python -c "import anthropic; print('Tudo certo!')"
```---

## Step 3 — Read the complete agent.py and understand each function

**Estimated time: 20 minutes**

Open the`agent.py`and read **each section** paying attention to the comments.

### What to look for in each section

**Section`tools`(lines ~20–61):**
- Each tool is a dictionary with`name`, `description` e `input_schema`- The`input_schema`follows the JSON Schema standard
- The`description`is read by the model to decide when to use the tool

**Section`executar_ferramenta`(lines ~67–86):**
- It is the dispatcher: receives the name and inputs, executes the right function
-`eval()`is used to calculate mathematical expressions (be careful in production!)
-`notas`is a global dictionary — simple memory in RAM

**Section`executar_agente`(lines ~90–146):**
- Maintains the list`mensagens`which grows with each iteration
-`stop_reason == "end_turn"`means the model is finished
-`stop_reason == "tool_use"`means the model wants to use a tool
- Tool results are added back to history

### Questions for reflection

1. What happens if the model calls a tool that does not exist in the dispatcher?
2. Why`mensagens`does it need to grow with each iteration and not just the last response is sent?
3. What does the property`tool_use_id`represents and why is it necessary?

---

## Step 4 — Run the agent and ask 3 different questions

**Estimated time: 15 minutes**```bash
python agent.py
```### Suggested questions to test each tool

**Testing`calcular`:**
```
Você: Qual é o resultado de 1234 * 5678?
```**Testing`salvar_nota` e `buscar_na_web`:**
```
Você: Busque informações sobre Python e salve um resumo como nota chamada "python"
```**Testing multi-step reasoning:**```
Você: Calcule quanto é 15% de 4800 e salve o resultado como "desconto"
```### What to watch out for while running

- How many iterations did the agent use to answer each question?
- Did the model always choose the right tool?
- What appears between`[Iteração N]` e `RESPOSTA FINAL`?

---

## Step 5 — Add the tool`get_weather`**Estimated time: 30 minutes**

The tool`get_weather`is already implemented in`tools/weather_tool.py`. Your task is to integrate it with the main agent.

### 5.1 — Examine the existing tool```bash
cat tools/weather_tool.py
```Understand:
- What`get_weather_tool_schema()`returns?
- What`get_weather(cidade)`do when there is no API key?

### 5.2 — Integrate with agent.py

Abra`agent.py`and make the following changes:

**At the beginning of the file, add import:**```python
from tools import get_weather_tool_schema, get_weather
```**On the list`tools`, add schema:**```python
tools = [
    # ... ferramentas existentes ...
    get_weather_tool_schema(),
]
```**No dispatcher`executar_ferramenta`, add case:**```python
elif nome == "consultar_clima":
    return get_weather(inputs["cidade"])
```### 5.3 — Test the new tool```bash
python agent.py
```

```
Você: Como está o clima em São Paulo hoje?
Você: Compara o clima de Curitiba com o de Manaus
```### 5.4 — (Optional) Configure the actual API

Create a free account on [OpenWeatherMap](https://openweathermap.org/api) and add to`.env`:

```
OPENWEATHER_API_KEY=sua_chave_aqui
```Also install the`requests`:
```bash
pip install requests
```---

## Step 6 — Add persistent memory with JSON file

**Estimated time: 45 minutes**

Currently, the agent's notes are lost when the program closes (they are in a dictionary in RAM). In this step, you will persist the notes into a JSON file.

### 6.1 — Understand the problem

Run the agent, save a note, close with Ctrl+C, and open again. The note is gone. Why?

### 6.2 — Implement persistence

Replace the dictionary`notas = {}`by functions that read and write to disk:```python
import pathlib

ARQUIVO_NOTAS = pathlib.Path("notas.json")

def carregar_notas() -> dict:
    """Load notes from disk, returning empty dict if file doesn't exist."""
    if ARQUIVO_NOTAS.exists():
        import json
        return json.loads(ARQUIVO_NOTAS.read_text(encoding="utf-8"))
    return {}

def salvar_notas_em_disco(notas: dict) -> None:
    """Persist the notes dictionary to disk as JSON."""
    import json
    ARQUIVO_NOTAS.write_text(
        json.dumps(notas, ensure_ascii=False, indent=2),
        encoding="utf-8"
    )

# Inicializa carregando do disco
notas = carregar_notas()
```### 6.3 — Update the dispatcher

Modify the case`salvar_nota`to persist to disk:```python
elif nome == "salvar_nota":
    notas[inputs["titulo"]] = inputs["conteudo"]
    salvar_notas_em_disco(notas)
    return f"Nota '{inputs['titulo']}' salva com sucesso."
```### 6.4 — Add the tool`listar_notas`Add to`tools`:

```python
{
    "name": "listar_notas",
    "description": "Lista todas as notas salvas anteriormente",
    "input_schema": {
        "type": "object",
        "properties": {},
        "required": []
    }
}
```And to the dispatcher:```python
elif nome == "listar_notas":
    if not notas:
        return "Nenhuma nota salva ainda."
    return "\n".join(f"- {titulo}: {conteudo}" for titulo, conteudo in notas.items())
```### 6.5 — Test persistence```bash
python agent.py
# Salve uma nota, feche com Ctrl+C
python agent.py
# Pergunte: "Quais notas eu tenho salvas?"
```---

## Step 7 — Final Challenge: Create a Complete Search Agent

**Estimated time: 2 hours**

This is the final challenge. You will build an agent capable of searching, synthesizing and reporting information on any topic.

### Objective

Create an agent that, given a research topic, is capable of:
1. Break the topic into subtopics
2. Search for information for each subtopic
3. Make relevant calculations (e.g. statistics)
4. Save the final report as a Markdown file

### Tools to implement

**`buscar_wikipedia(termo)`— real search on Wikipedia:**```python
# Instale: pip install wikipedia-api
import wikipediaapi

def buscar_wikipedia(termo: str, idioma: str = "pt") -> str:
    wiki = wikipediaapi.Wikipedia(
        user_agent="agente-pesquisa/1.0",
        language=idioma
    )
    pagina = wiki.page(termo)
    if not pagina.exists():
        return f"Artigo '{termo}' não encontrado na Wikipedia."
    # Return first 1000 chars to avoid overwhelming the context
    return pagina.summary[:1000]
```

**`salvar_relatorio(nome_arquivo, conteudo)`— saves Markdown file:**```python
def salvar_relatorio(nome_arquivo: str, conteudo: str) -> str:
    import pathlib
    caminho = pathlib.Path(nome_arquivo)
    if not nome_arquivo.endswith(".md"):
        caminho = pathlib.Path(nome_arquivo + ".md")
    caminho.write_text(conteudo, encoding="utf-8")
    return f"Relatório salvo em '{caminho}'."
```### Specialized system prompt

Replace the`system` do `client.messages.create`put:```python
system = """Você é um agente de pesquisa especializado. Quando receber um tema:
1. Identifique 3-5 subtópicos relevantes
2. Use buscar_wikipedia para pesquisar cada subtópico
3. Use calcular para qualquer estatística ou comparação numérica relevante
4. Sintetize as informações em um relatório estruturado
5. Salve o relatório final com salvar_relatorio
Seja metódico, cite as fontes e organize o relatório com títulos Markdown."""
```### Example of expected usage```
Você: Pesquise sobre energia solar no Brasil e gere um relatório
```The agent must:
- Search for "solar energy Brazil", "energy matrix Brazil", "solar installed capacity"
- Calculate comparisons (e.g. percentage growth)
- Generate and save a file`relatorio-energia-solar.md`### Success criteria

- [ ] The agent completes the search without human intervention
- [ ] The saved report has at least 3 sections with actual content
- [ ] The agent used at least 3 different tool calls
- [ ] The generated Markdown file is readable and well formatted

### Tips

- Increase`max_iteracoes`for 20 in this challenge
- Add`print`extras to understand the agent's reasoning
- If the search gets stuck, try a more specific topic

---

## Congratulations!

If you made it this far, you:

- Understood the inner workings of an agentic loop
- Implemented and integrated new tools
- Added data persistence
- Built a functional search agent

**Next project:** Project 2 — Agent with LangChain, where we will see how high-level frameworks abstract exactly what you just built by hand.