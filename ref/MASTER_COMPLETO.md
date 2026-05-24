# COMPLETE MASTER - Course Template

> **SINGLE DEFINITIVE DOCUMENT** - Everything you need to know to create pages
> **Version:** 2.0 | **Date:** 2026-01-20

---

## INDEX

1. [CRITICAL ERRORS - NEVER MAKE](#1-critical-errors---never-make)
2. [PROJECT STRUCTURE](#2-project-structure)
3. [COLOR SYSTEM](#3-color-system)
4. [TYPOGRAPHY AND TONE OF VOICE](#4-typography-and-tone-of-voice)
5. [GLOBAL NAVIGATION](#5-global-navigation)
6. [PAGE STRUCTURE](#6-page-structure)
7. [COMPONENTS](#7-components)
8. [COMPLETE HTML TEMPLATES](#8-templates-html-completos)
9. [JAVASCRIPT](#9-javascript)
10. [CSS REQUIRED](#10-css-required)

---

# 1. CRITICAL ERRORS - NEVER MAKE

## 1.1 BUTTONS ALWAYS ON THE LEFT```html
<!-- CORRETO -->
<div class="flex justify-start space-x-3">

<!-- ERRADO - NUNCA USAR -->
<div class="flex justify-center space-x-3">
```## 1.2 TOPICS WITH CIRCLE NUMBERS (NOT ARROWS!)```html
<!-- CORRETO - Numero em circulo -->
<span class="w-6 h-6 rounded-full bg-emerald-500/20 text-emerald-400 text-sm font-bold flex items-center justify-center">1</span>

<!-- ERRADO - Seta -->
<span class="text-emerald-400">▶</span>
```## 1.3 THREE MANDATORY SECTIONS PER TOPIC

Each expandable topic MUST have these 3 sections:
- **What it is:** Clear definition
- **Why learn:** Justification and benefits
- **Key concepts:** Main points

## 1.4 duclub ON ALL PAGES```html
<a href="https://duclub" target="_blank" class="text-sky-400 hover:text-sky-300 text-sm font-medium">duclub</a>
```- **COLOR:**`text-sky-400`(light blue)
- **POSITION:** Next to the logo, separated by`|`- **MANDATORY:** Index, tracks AND modules

## 1.5 LIGHT MODE CSS REQUIRED

Add to ALL pages (index, tracks AND modules):```css
/* Light mode overrides */
html:not(.dark) body {
  background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 50%, #f1f5f9 100%);
}
html:not(.dark) .bg-dark-900 { background-color: #ffffff; }
html:not(.dark) .bg-dark-800 { background-color: #f9fafb; }
html:not(.dark) .bg-dark-700 { background-color: #f3f4f6; }
html:not(.dark) .bg-dark-600 { background-color: #e5e7eb; }
html:not(.dark) .text-neutral-100 { color: #111827; }
html:not(.dark) .text-neutral-300 { color: #4b5563; }
html:not(.dark) .text-neutral-400 { color: #6b7280; }
html:not(.dark) .text-neutral-500 { color: #9ca3af; }
html:not(.dark) .border-dark-600 { border-color: #d1d5db; }
html:not(.dark) .border-dark-700 { border-color: #e5e7eb; }
```## 1.6 MODULE TITLE WITH TEXT-2XL```html
<!-- CORRETO -->
<h3 class="text-2xl font-bold mb-2">Titulo do Modulo</h3>

<!-- ERRADO - muito pequeno -->
<h3 class="text-lg font-bold mb-2">Titulo do Modulo</h3>
```---

# 2. PROJECT STRUCTURE

## 2.1 Standard Structure```
[NOME DO CURSO]
├── [EMOJI DO CURSO]
├── X Trilhas (definir quantidade)
│   ├── T1: [Nome] (Emerald) - X modulos
│   ├── T2: [Nome] (Blue) - X modulos
│   ├── T3: [Nome] (Purple) - X modulos
│   ├── T4: [Nome] (Amber) - X modulos (opcional)
│   ├── T5: [Nome] (Teal) - X modulos (opcional)
│   └── T6: [Nome] (Rose) - X modulos (opcional)
├── Total de Modulos
├── 6-8 Topicos por Modulo
└── Total de Topicos
```## 2.2 File Structure```
projeto/
├── index.html                    # Landing page
├── curso/
│   ├── trilha1/                  # Primeira trilha
│   │   ├── index.html            # Index da trilha
│   │   ├── modulo-1-1.html       # Modulo completo
│   │   ├── modulo-1-2.html
│   │   └── ...
│   ├── trilha2/
│   ├── trilha3/
│   └── ...
├── doc/                          # Documentacao
└── ref/                          # Referencias detalhadas
```---

# 3. COLOR SYSTEM

## 3.1 Colors per Track

| Trail | Color | text-* | bg-*/20 | border-*/30 | from-*/30 |
|--------|-----|--------|---------|-------------|-----------|
| T1 | Emerald |`text-emerald-400` | `bg-emerald-500/20` | `border-emerald-500/30` | `from-emerald-900/30`|
| T2 | Blue |`text-blue-400` | `bg-blue-500/20` | `border-blue-500/30` | `from-blue-900/30`|
| T3 | Purple |`text-purple-400` | `bg-purple-500/20` | `border-purple-500/30` | `from-purple-900/30`|
| T4 | Amber |`text-amber-400` | `bg-amber-500/20` | `border-amber-500/30` | `from-amber-900/30`|
| T5 | Teal |`text-teal-400` | `bg-teal-500/20` | `border-teal-500/30` | `from-teal-900/30`|
| T6 | Rose |`text-rose-400` | `bg-rose-500/20` | `border-rose-500/30` | `from-rose-900/30`|

## 3.2 Special Colors

| Color | Code | Class | Usage |
|-----|--------|--------|-----|
| Primary (Yellow) |`#FACC15` | `text-primary`, `bg-primary`| Logo, CTAs, highlights, tips |
| Sky |`#38bdf8` | `text-sky-400`| Link duclub |
| Red |`#f87171` | `text-red-400`| "Not to do", errors, warnings |
| Neutral 300 |`#d4d4d4` | `text-neutral-300`| Main text |
| Neutral 400 |`#a3a3a3` | `text-neutral-400`| Secondary text |

## 3.3 Dark Mode Palette

| Name | Code | Class | Usage |
|------|--------|--------|-----|
| dark-900 |`#111827` | `bg-dark-900`| Main fund |
| dark-800 |`#1f2937` | `bg-dark-800`| Cards, containers |
| dark-700 |`#374151` | `bg-dark-700`| Internal elements |
| dark-600 |`#4b5563` | `bg-dark-600`| Borders, dividers |

## 3.4 Light Mode Palette

| Name | Code | Usage |
|------|-----------|-----|
| white |`#ffffff`| Main background (dark-900) |
| gray-50 |`#f9fafb`| Cards (dark-800) |
| gray-100 |`#f3f4f6`| Elements (dark-700) |
| gray-200 |`#e5e7eb`| Edges (dark-600) |

---

# 4. TYPOGRAPHY AND TONE OF VOICE

## 4.1 Source```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
```

```css
body { font-family: 'Inter', sans-serif; }
```## 4.2 Hierarchy

| Element | Classes | Usage |
|----------|------------|-----|
| H1 (Hero) |`text-4xl sm:text-5xl font-bold`| Main page title |
| H2 (Section) |`text-2xl font-bold`| Section titles, topics |
| H3 (Subsection) |`text-xl font-bold` ou `text-lg font-semibold`| Subtitles, boxes |
| Body |`text-neutral-300`| Main text |
| Secondary |`text-neutral-400`| Secondary text |
| Small |`text-sm text-neutral-400`| Subtitles, metadata |

## 4.3 Tone of Voice

- **Direct and practical:** No fluff, gets to the point
- **Professional:** Focus on business application
- **Confident:** Clear statements, without “maybe” or “could be”
- **Didactic:** Step-by-step explanations

## 4.4 Paragraph Structure

- **Maximum 3-4 lines** per paragraph
- **First sentence:** Main concept
- **Highlight:** Keyword in **bold** or trail color

## 4.5 Use of Lists

- **Bullets:** For items in no specific order
- **Numbers (1, 2, 3):** For sequential steps
- **Check/X:** To do/not to do

---

# 5. GLOBAL NAVIGATION

## 5.1 Mandatory Elements

1. **Logo** (emoji + name) -`text-yellow-400`2. **duclub link** -`text-sky-400`3. **Trails** with full names (mobile: T1, T2, T3...)
4. **Theme Toggle** - dark/light button

## 5.2 Template Navigation```html
<nav class="sticky top-0 z-50 bg-dark-900/95 backdrop-blur-sm border-b border-dark-600">
  <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="flex justify-between items-center h-14">
      <!-- Logo + duclub -->
      <div class="flex items-center space-x-4">
        <a href="../../index.html" class="flex items-center space-x-2 text-yellow-400 hover:text-yellow-300">
          <span class="text-2xl">[EMOJI]</span>
          <span class="font-bold text-lg hidden sm:inline">[NOME DO CURSO]</span>
        </a>
        <span class="text-neutral-600">|</span>
        <a href="https://duclub" target="_blank" class="text-sky-400 hover:text-sky-300 text-sm font-medium">duclub</a>
      </div>

      <!-- Trilhas + Theme Toggle -->
      <div class="flex items-center space-x-1 sm:space-x-2">
        <!-- Trilha 1 (ativa) -->
        <a href="index.html" class="px-3 py-1.5 rounded-lg text-sm font-semibold text-emerald-400 bg-emerald-500/10">
          <span class="sm:hidden">T1</span>
          <span class="hidden sm:inline">[Nome Trilha 1]</span>
        </a>
        <!-- Trilha 2 (inativa) -->
        <a href="../trilha2/index.html" class="px-3 py-1.5 rounded-lg text-sm font-semibold text-neutral-400 hover:text-blue-400 hover:bg-blue-500/10 transition-colors">
          <span class="sm:hidden">T2</span>
          <span class="hidden sm:inline">[Nome Trilha 2]</span>
        </a>
        <!-- Adicionar mais trilhas conforme necessario -->

        <!-- Theme Toggle -->
        <button id="theme-toggle" class="p-2 rounded-lg bg-dark-700 hover:bg-dark-600 transition-colors ml-2">
          <svg id="theme-toggle-dark-icon" class="hidden w-5 h-5 text-neutral-300" fill="currentColor" viewBox="0 0 20 20">
            <path d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z"></path>
          </svg>
          <svg id="theme-toggle-light-icon" class="hidden w-5 h-5 text-neutral-300" fill="currentColor" viewBox="0 0 20 20">
            <path d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 000 2h1z" fill-rule="evenodd" clip-rule="evenodd"></path>
          </svg>
        </button>
      </div>
    </div>
  </div>
</nav>
```## 5.3 Track Link Colors

| Status | Classes |
|--------|---------|
| Active |`text-[cor]-400 bg-[cor]-500/10`|
| Inactive |`text-neutral-400 hover:text-[cor]-400 hover:bg-[cor]-500/10 transition-colors`|

---

# 6. PAGE STRUCTURE

## 6.1 Track Page (Index)```
TRILHA INDEX
├── Navigation Global
│   ├── Logo (emoji + nome) - yellow-400
│   ├── duclub - sky-400
│   ├── Trilhas (nomes completos, cor da ativa)
│   └── Theme Toggle
├── Header com Gradiente
│   ├── Badge (TRILHA X) - cor da trilha
│   ├── Titulo + emoji
│   ├── Descricao
│   └── Stats Grid (4 colunas: Modulos, Topicos, Duracao, Nivel)
├── Navegacao Rapida (grid de cards) - OPCIONAL
├── Divisor "Conteudo Detalhado"
├── Cards de Modulo (repetido)
│   ├── Header
│   │   ├── Numero do modulo (X.X) - cor da trilha
│   │   ├── Duracao (~XX min)
│   │   ├── Titulo com emoji - TEXT-2XL
│   │   └── Descricao
│   ├── Topicos Expansiveis (6-8 por modulo)
│   │   ├── NUMERO em circulo (NAO seta)
│   │   ├── Emoji + titulo + subtitulo
│   │   └── 3 secoes: O que e / Por que / Conceitos
│   └── Botoes (ESQUERDA - justify-start)
│       ├── Ver em Modal
│       └── Ver Completo
├── Navegacao (Voltar | Proxima Trilha)
└── Footer
```## 6.2 Module Page```
MODULO COMPLETO
├── Navigation Global (identico ao index)
├── Breadcrumb (Inicio / Trilha X / Modulo X.X)
├── Header
│   ├── Badge (MODULO X.X)
│   ├── Titulo + emoji
│   ├── Descricao
│   └── Stats (Topicos, Minutos, Nivel, Tipo)
├── Topicos (6 secoes ricas)
│   └── Section por topico
│       ├── Numero em circulo GRANDE (w-12 h-12)
│       ├── Titulo h2
│       ├── Paragrafo introdutorio
│       └── Boxes variados:
│           ├── Conceito Principal (gradiente da trilha)
│           ├── Dados/Pesquisa (blue)
│           ├── Dica Pratica (primary/yellow)
│           ├── Grid Fazer vs Evitar (emerald/red)
│           ├── Timeline de casos/passos
│           └── Box de alerta (red) - quando necessario
├── Resumo Final
│   ├── Checklist do que aprendemos
│   ├── Proximo modulo
│   └── CTAs de navegacao
└── Footer
```## 6.3 Standard Spacing

| Class | Usage |
|--------|-----|
|`mb-16`| Between main sections (topics) |
|`mb-12`| Before the final summary |
|`mb-8`| Between content groups |
|`mb-6`| Between boxes within section |
|`mb-4`| Between smaller elements |
|`p-8`| Main card padding |
|`p-6`| Internal box padding |
|`p-4`| Padding of minor elements |
|`gap-4`| Stats grid |
|`gap-6`| Comparison grid |

---

# 7. COMPONENTS

## 7.1 Number in Circle

**Small (expandable topics):**```html
<span class="w-6 h-6 rounded-full bg-emerald-500/20 text-emerald-400 text-sm font-bold flex items-center justify-center flex-shrink-0">1</span>
```**Large (module sections):**```html
<span class="flex items-center justify-center w-12 h-12 rounded-full bg-emerald-500/20 text-emerald-400 font-bold text-xl">1</span>
```## 7.2 Expandable Topic with 3 Sections```html
<div class="topic-item">
  <button onclick="toggleTopic(this)" class="w-full px-6 py-4 flex items-center space-x-3 hover:bg-dark-700/50 transition-colors text-left">
    <span class="w-6 h-6 rounded-full bg-emerald-500/20 text-emerald-400 text-sm font-bold flex items-center justify-center flex-shrink-0">1</span>
    <span class="text-lg">[EMOJI]</span>
    <div>
      <span class="font-medium">Titulo do Topico</span>
      <span class="text-neutral-500 text-sm ml-2">- Subtitulo</span>
    </div>
  </button>
  <div class="topic-explanation px-6 pb-4">
    <div class="bg-dark-700/50 rounded-lg p-4 space-y-3 ml-9">
      <div>
        <span class="text-emerald-400 font-semibold">O que e:</span>
        <p class="text-neutral-300 text-sm">Definicao clara e objetiva.</p>
      </div>
      <div>
        <span class="text-emerald-400 font-semibold">Por que aprender:</span>
        <p class="text-neutral-300 text-sm">Justificativa e beneficios praticos.</p>
      </div>
      <div>
        <span class="text-emerald-400 font-semibold">Conceitos-chave:</span>
        <p class="text-neutral-300 text-sm">Pontos principais a memorizar.</p>
      </div>
    </div>
  </div>
</div>
```## 7.3 Module Card```html
<div class="bg-dark-800 rounded-xl border border-dark-600 mb-6">
  <!-- Header do Modulo -->
  <div class="p-6 border-b border-dark-600">
    <div class="flex items-center justify-between mb-2">
      <span class="text-emerald-400 font-bold">1.1</span>
      <span class="text-xs text-neutral-500">~30 min</span>
    </div>
    <h3 class="text-2xl font-bold mb-2">[EMOJI] Titulo do Modulo</h3>
    <p class="text-neutral-400 text-sm">Descricao breve do modulo.</p>
  </div>

  <!-- Topicos Expansiveis -->
  <div class="divide-y divide-dark-600">
    <!-- Topicos aqui -->
  </div>

  <!-- Botoes (ESQUERDA!) -->
  <div class="p-4 bg-dark-700/30 flex justify-start space-x-3">
    <button onclick="openModal('modal-1-1')" class="px-4 py-2 text-sm bg-dark-600 hover:bg-dark-500 rounded-lg transition-colors">
      Ver em Modal
    </button>
    <a href="modulo-1-1.html" class="px-4 py-2 text-sm bg-emerald-600 hover:bg-emerald-500 text-white rounded-lg transition-colors">
      Ver Completo
    </a>
  </div>
</div>
```## 7.4 Track Header```html
<header class="bg-gradient-to-br from-emerald-900/30 via-dark-800 to-dark-800 py-12 border-b border-dark-600">
  <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
    <span class="inline-block px-3 py-1 bg-emerald-500/20 text-emerald-400 text-xs font-semibold rounded-full mb-4">
      TRILHA 1
    </span>
    <h1 class="text-3xl sm:text-4xl font-bold mb-4">[EMOJI] [Nome da Trilha]</h1>
    <p class="text-lg text-neutral-400 max-w-3xl">
      Descricao da trilha aqui...
    </p>

    <!-- Stats Grid -->
    <div class="grid grid-cols-4 gap-4 mt-8 max-w-2xl">
      <div class="bg-dark-800/50 rounded-lg p-3 border border-dark-600">
        <div class="text-xl font-bold text-emerald-400">8</div>
        <div class="text-xs text-neutral-400">Modulos</div>
      </div>
      <div class="bg-dark-800/50 rounded-lg p-3 border border-dark-600">
        <div class="text-xl font-bold text-emerald-400">48</div>
        <div class="text-xs text-neutral-400">Topicos</div>
      </div>
      <div class="bg-dark-800/50 rounded-lg p-3 border border-dark-600">
        <div class="text-xl font-bold text-emerald-400">~4h</div>
        <div class="text-xs text-neutral-400">Duracao</div>
      </div>
      <div class="bg-dark-800/50 rounded-lg p-3 border border-dark-600">
        <div class="text-xl font-bold text-emerald-400">Basico</div>
        <div class="text-xs text-neutral-400">Nivel</div>
      </div>
    </div>
  </div>
</header>
```## 7.5 Module Header```html
<header class="bg-gradient-to-br from-emerald-900/30 via-dark-800 to-dark-800 py-12 border-b border-dark-600">
  <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
    <span class="inline-block px-3 py-1 bg-emerald-500/20 text-emerald-400 text-xs font-semibold rounded-full mb-4">
      MODULO 1.1
    </span>
    <h1 class="text-3xl sm:text-4xl font-bold mb-4">[EMOJI] Titulo do Modulo</h1>
    <p class="text-lg text-neutral-400 max-w-3xl">Descricao do modulo...</p>

    <!-- Stats Banner -->
    <div class="grid grid-cols-4 gap-4 mt-8 max-w-2xl">
      <div class="bg-dark-800/50 rounded-lg p-3 border border-dark-600">
        <div class="text-xl font-bold text-emerald-400">6</div>
        <div class="text-xs text-neutral-400">Topicos</div>
      </div>
      <div class="bg-dark-800/50 rounded-lg p-3 border border-dark-600">
        <div class="text-xl font-bold text-emerald-400">30</div>
        <div class="text-xs text-neutral-400">Minutos</div>
      </div>
      <div class="bg-dark-800/50 rounded-lg p-3 border border-dark-600">
        <div class="text-xl font-bold text-emerald-400">Basico</div>
        <div class="text-xs text-neutral-400">Nivel</div>
      </div>
      <div class="bg-dark-800/50 rounded-lg p-3 border border-dark-600">
        <div class="text-xl font-bold text-emerald-400">Teoria</div>
        <div class="text-xs text-neutral-400">Tipo</div>
      </div>
    </div>
  </div>
</header>
```## 7.6 Breadcrumb```html
<nav class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8 py-4">
  <div class="flex items-center space-x-2 text-sm text-neutral-400">
    <a href="../../index.html" class="hover:text-emerald-400">Inicio</a>
    <span>/</span>
    <a href="index.html" class="hover:text-emerald-400">Trilha 1</a>
    <span>/</span>
    <span class="text-emerald-400">Modulo 1.1</span>
  </div>
</nav>
```## 7.7 Topic Section (Module Page)```html
<section id="topico-1" class="mb-16">
  <div class="flex items-center space-x-4 mb-6">
    <span class="flex items-center justify-center w-12 h-12 rounded-full bg-emerald-500/20 text-emerald-400 font-bold text-xl">1</span>
    <h2 class="text-2xl font-bold">[EMOJI] Titulo do Topico</h2>
  </div>

  <p class="text-neutral-300 mb-6 leading-relaxed">
    Paragrafo introdutorio com <strong class="text-emerald-400">destaque</strong> nas palavras importantes...
  </p>

  <!-- Boxes de conteudo aqui -->
</section>
```## 7.8 Main Concept Box```html
<div class="bg-gradient-to-br from-emerald-900/30 to-dark-800 rounded-xl border border-emerald-500/30 p-6 mb-6">
  <h3 class="text-lg font-semibold text-emerald-400 mb-4 flex items-center">
    <span class="mr-2">[EMOJI]</span> Titulo
  </h3>
  <p class="text-neutral-300 mb-4">Texto explicativo...</p>
  <ul class="space-y-2 text-neutral-300">
    <li class="flex items-start space-x-2">
      <span class="text-emerald-400 mt-1">•</span>
      <span>Item da lista</span>
    </li>
  </ul>
</div>
```## 7.9 Box Data/Search```html
<div class="bg-blue-900/20 rounded-xl border border-blue-500/30 p-6 mb-6">
  <h3 class="text-lg font-semibold text-blue-400 mb-4 flex items-center">
    <span class="mr-2">[EMOJI]</span> Dados de Pesquisa
  </h3>
  <ul class="space-y-2 text-neutral-300">
    <li><strong>XX%</strong> - Estatistica importante</li>
  </ul>
</div>
```## 7.10 Practical Tip Box```html
<div class="bg-primary/10 rounded-xl border border-primary/30 p-6 mb-6">
  <h3 class="text-lg font-semibold text-primary mb-3 flex items-center">
    <span class="mr-2">[EMOJI]</span> Dica Pratica
  </h3>
  <p class="text-neutral-300">Texto da dica...</p>
</div>
```## 7.11 Grid Do vs Avoid```html
<div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
  <div class="bg-emerald-900/20 rounded-xl border border-emerald-500/30 p-6">
    <h4 class="font-bold text-emerald-400 mb-4">✓ O que FAZER</h4>
    <ul class="space-y-3 text-neutral-300">
      <li class="flex items-start space-x-2">
        <span class="text-emerald-400">✓</span>
        <span>Item positivo</span>
      </li>
    </ul>
  </div>
  <div class="bg-red-900/20 rounded-xl border border-red-500/30 p-6">
    <h4 class="font-bold text-red-400 mb-4">✗ O que NAO fazer</h4>
    <ul class="space-y-3 text-neutral-300">
      <li class="flex items-start space-x-2">
        <span class="text-red-400">✗</span>
        <span>Item negativo</span>
      </li>
    </ul>
  </div>
</div>
```## 7.12 Timeline```html
<div class="space-y-6 mb-6">
  <div class="flex items-start space-x-4">
    <div class="flex-shrink-0 w-12 h-12 rounded-full bg-emerald-500/20 flex items-center justify-center">
      <span class="text-emerald-400 font-bold">1</span>
    </div>
    <div class="flex-1 bg-dark-800 rounded-xl p-6 border border-dark-600">
      <h4 class="font-semibold text-white mb-2">Titulo do Passo</h4>
      <p class="text-sm text-neutral-400 mb-3">Contexto</p>
      <p class="text-neutral-300 text-sm">Descricao detalhada...</p>
    </div>
  </div>
</div>
```## 7.13 Alert Box```html
<div class="bg-red-900/20 rounded-xl border border-red-500/30 p-6 mb-6">
  <h3 class="text-lg font-semibold text-red-400 mb-3 flex items-center">
    <span class="mr-2">[EMOJI]</span> Atencao
  </h3>
  <p class="text-neutral-300">Texto de alerta...</p>
</div>
```## 7.14 Module Summary```html
<section class="mb-12">
  <div class="bg-gradient-to-br from-emerald-900/40 via-dark-800 to-dark-800 rounded-xl border border-emerald-500/30 p-8">
    <h2 class="text-2xl font-bold mb-6 flex items-center">
      <span class="mr-3">[EMOJI]</span> Resumo do Modulo
    </h2>

    <div class="space-y-4 mb-8">
      <div class="flex items-start space-x-3">
        <span class="text-emerald-400 mt-1">✓</span>
        <div>
          <strong class="text-white">Ponto principal</strong>
          <span class="text-neutral-400"> - Explicacao breve</span>
        </div>
      </div>
      <!-- Mais pontos... -->
    </div>

    <div class="bg-dark-800/50 rounded-lg p-4 mb-8">
      <h3 class="font-semibold text-emerald-400 mb-2">Proximo Modulo:</h3>
      <p class="text-neutral-300">1.2 - Titulo do Proximo Modulo</p>
    </div>

    <div class="flex flex-col sm:flex-row gap-4">
      <a href="index.html" class="flex-1 text-center px-6 py-3 bg-dark-700 text-neutral-300 rounded-lg font-semibold hover:bg-dark-600 transition-colors">
        ← Voltar para Trilha
      </a>
      <a href="modulo-1-2.html" class="flex-1 text-center px-6 py-3 bg-emerald-600 text-white rounded-lg font-semibold hover:bg-emerald-500 transition-colors">
        Proximo Modulo →
      </a>
    </div>
  </div>
</section>
```---

# 8. COMPLETE HTML TEMPLATES

## 8.1 HTML base (use on ALL pages)```html
<!DOCTYPE html>
<html lang="pt-BR" class="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Titulo] | [Nome do Curso]</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          colors: {
            primary: '#FACC15',
            dark: { 900: '#111827', 800: '#1f2937', 700: '#374151', 600: '#4b5563' }
          }
        }
      }
    }
  </script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <style>
    body { font-family: 'Inter', sans-serif; }
    .dark body { background-color: #111827; }
    .topic-explanation { display: none; }
    .topic-explanation.active { display: block; }

    /* Light mode overrides - OBRIGATORIO */
    html:not(.dark) body {
      background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 50%, #f1f5f9 100%);
    }
    html:not(.dark) .bg-dark-900 { background-color: #ffffff; }
    html:not(.dark) .bg-dark-800 { background-color: #f9fafb; }
    html:not(.dark) .bg-dark-700 { background-color: #f3f4f6; }
    html:not(.dark) .bg-dark-600 { background-color: #e5e7eb; }
    html:not(.dark) .text-neutral-100 { color: #111827; }
    html:not(.dark) .text-neutral-300 { color: #4b5563; }
    html:not(.dark) .text-neutral-400 { color: #6b7280; }
    html:not(.dark) .text-neutral-500 { color: #9ca3af; }
    html:not(.dark) .border-dark-600 { border-color: #d1d5db; }
    html:not(.dark) .border-dark-700 { border-color: #e5e7eb; }
  </style>
</head>
<body class="bg-dark-900 text-neutral-100 min-h-screen">

  <!-- NAVIGATION -->
  <nav class="sticky top-0 z-50 bg-dark-900/95 backdrop-blur-sm border-b border-dark-600">
    <!-- Ver secao 5.2 -->
  </nav>

  <!-- CONTEUDO -->
  <main class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
    <!-- Conteudo aqui -->
  </main>

  <!-- FOOTER -->
  <footer class="border-t border-dark-600 py-8 mt-16">
    <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8 text-center text-neutral-500 text-sm">
      <p>[Nome do Curso] - [Ano]</p>
    </div>
  </footer>

  <!-- SCRIPTS -->
  <script>
    // Ver secao 9
  </script>

</body>
</html>
```---

# 9. JAVASCRIPT

## 9.1 Topic Toggle```javascript
function toggleTopic(button) {
  const topicItem = button.closest('.topic-item');
  const explanation = topicItem.querySelector('.topic-explanation');
  const moduleCard = button.closest('.bg-dark-800');

  // Fecha outros topicos do mesmo modulo
  moduleCard.querySelectorAll('.topic-explanation.active').forEach(exp => {
    if (exp !== explanation) {
      exp.classList.remove('active');
    }
  });

  // Toggle do topico atual
  explanation.classList.toggle('active');
}
```## 9.2 Theme Toggle```javascript
const themeToggle = document.getElementById('theme-toggle');
const themeToggleDarkIcon = document.getElementById('theme-toggle-dark-icon');
const themeToggleLightIcon = document.getElementById('theme-toggle-light-icon');
const html = document.documentElement;

// Inicializa icone correto
if (localStorage.getItem('theme') === 'light' || (!localStorage.getItem('theme') && !html.classList.contains('dark'))) {
  themeToggleDarkIcon.classList.remove('hidden');
  html.classList.remove('dark');
} else {
  themeToggleLightIcon.classList.remove('hidden');
}

// Click handler
themeToggle.addEventListener('click', () => {
  themeToggleDarkIcon.classList.toggle('hidden');
  themeToggleLightIcon.classList.toggle('hidden');
  html.classList.toggle('dark');
  localStorage.setItem('theme', html.classList.contains('dark') ? 'dark' : 'light');
});
```## 9.3 Modal with Iframe

**IMPORTANT:** The modal must load the module page via iframe to ensure that the content is IDENTICAL to that of the target page. DO NOT create duplicate or summarized content.

### Modal HTML Template```html
<div id="modal-1-1" class="modal hidden fixed inset-0 z-50 flex items-center justify-center p-2 sm:p-4 bg-black/80" onclick="if(event.target === this) closeModal()">
  <div class="bg-dark-800 rounded-xl w-full max-w-6xl h-[95vh] flex flex-col border border-dark-600">
    <!-- Header -->
    <div class="p-4 border-b border-dark-600 flex justify-between items-center flex-shrink-0">
      <div class="flex items-center space-x-3">
        <span class="text-emerald-400 font-bold">1.1</span>
        <span class="font-semibold">Titulo do Modulo</span>
      </div>
      <button onclick="closeModal()" class="text-neutral-400 hover:text-white text-2xl leading-none">&times;</button>
    </div>
    <!-- Iframe carrega a pagina completa do modulo -->
    <iframe src="modulo-1-1.html" class="flex-1 w-full rounded-b-xl"></iframe>
  </div>
</div>
```**Advantages of iframe:**
- Content always synchronized with the module page
- No duplication of code/content
- Simplified maintenance (updates in one place)

### Modal JavaScript```javascript
function openModal(modalId) {
  const modal = document.getElementById(modalId);
  if (modal) {
    modal.classList.remove('hidden');
    document.body.style.overflow = 'hidden';
  }
}

function closeModal() {
  document.querySelectorAll('.modal').forEach(modal => {
    modal.classList.add('hidden');
  });
  document.body.style.overflow = 'auto';
}

// Fechar com ESC
document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape') closeModal();
});
```---

# 10. MANDATORY CSS

## 10.1 CSS Base```css
body { font-family: 'Inter', sans-serif; }
.dark body { background-color: #111827; }
```## 10.2 Expandable Topics```css
.topic-explanation { display: none; }
.topic-explanation.active { display: block; }
```## 10.3 Light Mode (MANDATORY on ALL pages)```css
html:not(.dark) body {
  background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 50%, #f1f5f9 100%);
}
html:not(.dark) .bg-dark-900 { background-color: #ffffff; }
html:not(.dark) .bg-dark-800 { background-color: #f9fafb; }
html:not(.dark) .bg-dark-700 { background-color: #f3f4f6; }
html:not(.dark) .bg-dark-600 { background-color: #e5e7eb; }
html:not(.dark) .text-neutral-100 { color: #111827; }
html:not(.dark) .text-neutral-300 { color: #4b5563; }
html:not(.dark) .text-neutral-400 { color: #6b7280; }
html:not(.dark) .text-neutral-500 { color: #9ca3af; }
html:not(.dark) .border-dark-600 { border-color: #d1d5db; }
html:not(.dark) .border-dark-700 { border-color: #e5e7eb; }
```---

# FINAL REMINDER

**ALWAYS check before finalizing any page:**

1. ✓ Buttons on the LEFT (justify-start)
2. ✓ Circled numbers (not arrows)
3. ✓ 3 sections per topic
4. ✓ duclub present (sky-400)
5. ✓ Light mode CSS included
6. ✓ Module title with text-2xl
7. ✓ Correct track colors
8. ✓ Theme toggle working

---

**Last update:** 2026-01-20
**Version:** 1.0