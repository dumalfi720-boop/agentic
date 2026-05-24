# REVISION CHECKLIST

> **Use this document to check that everything has been implemented correctly**
> **Version:** 1.5 | **Date:** 2026-01-21

---

## HOW TO USE

1. Copy this checklist for each page you create/review
2. Check each item as you check
3. DO NOT publish until all items are checked

---

# CHECKLIST TRACK PAGE (INDEX)

## 1. CRITICAL ERRORS (MANDATORY)

- [ ] **Buttons on the LEFT** - Check`justify-start`(NOT center)
- [ ] **Numbers in circles** - DO NOT use arrows in topics
- [ ] **3 sections per topic** - Each topic has: What is / Why to learn / Key concepts
- [ ] **duclub present** - Link with`text-sky-400`next to the logo
- [ ] **Light mode CSS** - Complete block of overrides in`<style>`- [ ] **Module title** - Using`text-2xl font-bold`- [ ] **ALL complete modules** - ALL modules (1.1 to 1.8) must have expandable topics, NOT just header+button
- [ ] **Modal present** - "View in Modal" button + modal HTML + JavaScript (openModal/closeModal)
- [ ] **Modal with iframe** - Modal must use`<iframe src="modulo-X-X.html">`to load the complete page (NO duplicate content)
- [ ] **2 module sections** - First simple cards (grid 2 cols), then detailed cards with topics
- [ ] **Simple clickable cards** - Section 1 uses`<a>`clickable (WITHOUT buttons), with hover on the edge and title

## 2. NAVIGATION

- [ ] Logo with correct project emoji
- [ ] Visible course name (hidden sm:inline on mobile)
- [ ] Tab`|`between logo and duclub
- [ ] Link duclub com`text-sky-400 hover:text-sky-300`- [ ] All trails listed with full names
- [ ] Current track with active color (`text-[cor]-400 bg-[cor]-500/10`)
- [ ] Other tracks with correct hover
- [ ] Theme toggle present with SVG icons
- [ ] Nav sticky com`backdrop-blur-sm`## 3. HEADER

- [ ] Correct track gradient (`from-[cor]-900/30`)
- [ ] "TRAIL X" badge with correct color
- [ ] Title with emoji
- [ ] Trail description
- [ ] Stats Grid with 4 columns:
  - [ ] Modules
  - [ ] Topics
  - [ ] Duration
  - [ ] Level

## 4. MODULE CARDS

**ATTENTION:** Check that ALL 8 modules are complete (not just some with topics and others with just header+button)

For each module check:

- [ ] Module number (X.X) with track color
- [ ] Duration (~XX min)
- [ ] Title with emoji and`text-2xl`- [ ] Brief description
- [ ] 6-8 expandable topics
- [ ] Each topic with a number in a circle (NOT arrow)
- [ ] Each topic with emoji + title + subtitle
- [ ] Expandable content with 3 sections
- [ ] Buttons aligned to the LEFT

## 5. FUNCTIONALITY

- [ ] Topic toggle working (open/close)
- [ ] Only 1 open topic per module
- [ ] Theme toggle changing dark/light
- [ ] Light mode rendering correctly
- [ ] Correct button links

## 6. RESPONSIVITY

- [ ] Mobile: Trails show T1, T2, T3...
- [ ] Desktop: Tracks show full names
- [ ] Responsive grid stats
- [ ] Cards stack on mobile

---

# CHECKLIST MODULE PAGE

## 1. CRITICAL ERRORS (MANDATORY)

- [ ] **Navigation buttons** - Check alignment
- [ ] **Numbers in a BIG circle** -`w-12 h-12`in the sections
- [ ] **duclub present** - Link with`text-sky-400`- [ ] **Light mode CSS** - Complete block of overrides in`<style>`## 2. NAVIGATION

- [ ] Identifies the trail page
- [ ] Matching track marked as active

## 3. BREADCRUMB

- [ ] Present below navigation
- [ ] Format: Home / Track X / Module X.X
- [ ] Working links
- [ ] Current module highlighted with track color

## 4. HEADER

- [ ] Correct track gradient
- [ ] Badge "MODULO X.X"
- [ ] Title with emoji
- [ ] Module description
- [ ] Stats Grid with 4 columns:
  - [ ] Topics
  - [ ] Minutes
  - [ ] Level
  - [ ] Type

## 5. TOPICS (SECTIONS)

For each topic (1-6) check:

- [ ] Number in a BIG circle (`w-12 h-12`)
- [ ] H2 title with emoji
- [ ] Introductory paragraph
- [ ] Spacing`mb-16`between sections

### Content Boxes (check variety):

- [ ] Main Concept Box (track gradient)
- [ ] Data/Search Box (blue)
- [ ] Practice Tip Box (primary/yellow)
- [ ] Do vs Avoid Grid (emerald/red)
- [ ] Timeline (if applicable)
- [ ] Alert Box (red, if applicable)

## 6. FINAL SUMMARY

- [ ] Title with emoji
- [ ] Checklist of what was learned (items with ✓)
- [ ] Indication of the next module
- [ ] "Return to Track" button
- [ ] "Next Module" button
- [ ] Correct gradient of the track in the container

## 7. FUNCTIONALITY

- [ ] Theme toggle working
- [ ] Light mode rendering correctly
- [ ] Correct navigation links
- [ ] Topic IDs to anchor (#topic-1, #topic-2, etc.)

---

# CHECKLIST COLORS BY TRACK

Check if the track uses the correct colors according to the number:

## Track 1 (Emerald)

- [ ]`text-emerald-400`
- [ ] `bg-emerald-500/20`
- [ ] `border-emerald-500/30`
- [ ] `from-emerald-900/30`## Track 2 (Blue)

- [ ]`text-blue-400`
- [ ] `bg-blue-500/20`
- [ ] `border-blue-500/30`
- [ ] `from-blue-900/30`## Track 3 (Purple)

- [ ]`text-purple-400`
- [ ] `bg-purple-500/20`
- [ ] `border-purple-500/30`
- [ ] `from-purple-900/30`## Track 4 (Amber)

- [ ]`text-amber-400`
- [ ] `bg-amber-500/20`
- [ ] `border-amber-500/30`
- [ ] `from-amber-900/30`## Trail 5 (Teal)

- [ ]`text-teal-400`
- [ ] `bg-teal-500/20`
- [ ] `border-teal-500/30`
- [ ] `from-teal-900/30`## Track 6 (Rose)

- [ ]`text-rose-400`
- [ ] `bg-rose-500/20`
- [ ] `border-rose-500/30`
- [ ] `from-rose-900/30`---

# JAVASCRIPT CHECKLIST

- [ ] Function`toggleTopic()`gift
- [ ] Function closes other topics in the same module
- [ ] Theme toggle initializes correct icon
- [ ] Theme toggle saves preference in localStorage
- [ ] Theme toggle toggles class`dark`in html
- [ ] Function`openModal()`present (track page)
- [ ] Function`closeModal()`present (track page)
- [ ] Modal closes with ESC key

---

# CSS CHECKLIST

- [ ] Font Inter imported
- [ ]`body { font-family: 'Inter', sans-serif; }`
- [ ] `.topic-explanation { display: none; }`
- [ ] `.topic-explanation.active { display: block; }`- [ ] Complete block of Light mode overrides

---

# CHECKLIST SLIDE PAGE

## Structure
- [ ] Folder`/curso/trilhaX/slides/`created
- [ ] File`slides-X-X.html`for each module with images
- [ ] Referenced images from`/doc/XX/`(folder corresponds to the module)

##Layout
- [ ] Header with badge "MODULO X.X SLIDES"
- [ ] Stats: number of slides, estimated time, "Visual" type
- [ ] Slide cards with circled number + title
- [ ] Image clickable to zoom (modal)
- [ ] Navigation between slides and back to the module

## Content
- [ ] **COMPLETE AND DETAILED TEXT** - Each slide must have a rich description explaining the visual content
- [ ] Contextualization of what is being shown
- [ ] Key concepts highlighted in bold
- [ ] Connection to module content

## Functionality
- [ ] Zoom mode working (click on the image)
- [ ] Close modal with ESC
- [ ] Theme toggle working
- [ ] Light mode rendering

## Buttons (where to add)
- [ ] Purple button "📊 View Slides" on the module page (next to navigation)
- [ ] Purple "📊 Slides" button on the detailed track index card

## Folder Mapping
| Doc folder | Module |
|-----------|-----------|
| doc/11 | 1.1 |
| doc/12 | 1.2 |
| doc/13 | 1.3 |

---

# FINAL CHECK

Before publishing, respond:

1. [ ] Did I open the page in the browser?
2. [ ] Have I tested dark mode?
3. [ ] Have I tested the light mode?
4. [ ] Did I click on all expandable topics?
5. [ ] Did I check all the links?
6. [ ] Did I test on mobile (or resize the window)?
7. [ ] Have I visually compared it to an existing page, which is correct?

---

# QUICK TEMPLATE TO COPY```
## Revisao: [Nome da Pagina]
Data: ____/____/____

### Erros Criticos
- [ ] Botoes ESQUERDA
- [ ] Numeros em circulo
- [ ] 3 secoes por topico
- [ ] duclub
- [ ] Light mode CSS
- [ ] Titulo text-2xl
- [ ] Modulos completos (index)
- [ ] Modal funcionando (index)
- [ ] Cartoes simples clicaveis (index)

### Estrutura
- [ ] Navigation completa
- [ ] Header com stats
- [ ] Conteudo correto
- [ ] Resumo/navegacao

### Funcional
- [ ] Dark mode OK
- [ ] Light mode OK
- [ ] Links OK
- [ ] Responsivo OK

### Status: [ ] APROVADO / [ ] PENDENTE
```---

# MOST COMMON ERRORS (CHECK SPECIALLY)

| Error | What to check |
|------|-----------------|
| Centralized buttons | Search for`justify-center`on module buttons |
| Arrows instead of numbers | Search for`▶` ou `→`in topics |
| Missing duclub | Check navigation |
| Light mode broken | Test by switching the theme |
| Small title | Check if h3 of the module has`text-2xl`|
| Wrong color | Compare with track color table |
| Topic without 3 sections | Open each topic and check |
| Incomplete modules in the index | Check if ALL modules have expandable topics (not just header+button) |
| Missing modal | Check the "View in Modal" button, HTML of the modals and openModal/closeModal functions |
| Modal without iframe | Modal MUST use`<iframe src="modulo-X-X.html">`to load full page |
| Missing simple cards section | Index must have: 1) Simple cards (grid 2 cols) 2) Detailed cards with topics |
| Simple cards with buttons | Section 1 must use`<a>`clickable, NO buttons - just hover on border/title |

---

**Last update:** 2026-01-21
**Version:** 1.5