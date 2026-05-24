# EVALUATION REPORT — Agentic Engineering Practical Projects

> Detailed technical evaluation of the 4 practical projects
> Date: 2026-03-03 | Reviewer: Claude Opus 4.6

---

## EXECUTIVE SUMMARY

| Project | Note | Main Force | Main Problem |
|---------|------|-----------------|-------------------|
| P1 Agent of Zero | **8.5/10** | Functional code + 13 tests | eval() needs security reinforcement |
| P2 Claude Code | **8.0/10** | Skills + professional scripts | CLAUDE.md example is from another project |
| P3 Antigravity | **7.5/10** | Excellent documentation | Product in beta — instructions may not work |
| P4 Codex CLI | **8.0/10** | HOW_EXECUTAR.txt + GitHub Actions | Check if`@openai/codex`is available via npm |
| Menu (index.html) | **9.5/10** | Coherent design, comparative table | No critical issues |
| Prompts (prompts.html) | **9.5/10** | 5 specialized prompts + copy button | No critical issues |

**Total files:** 50 | **Total lines:** ~16,000+ | **ZIPs:** 4 (36-46KB each)

---

## PROJECT 1 — AGENT FROM ZERO (8.5/10)

### Strengths
-`agent.py`functional with correct agentic loop, 3 tools, clean dispatcher
- 13 unit tests passing (dispatcher + agentic loop with mock)
- Extensible architecture with`tools/weather_tool.py`as an example plug-and-play
-`ROTEIRO.md`excellent pedagogical — 7 progressive steps with estimated times
- Correct HTML design system: orange, duclub, light mode, numbered circles

### Problems Found

| Severity | Problem | Location |
|---|---|---|
| Media |`eval()`sandboxed but without input validation — possible DoS with`(2**2)**1000000` | `agent.py:77`|
| Low |`requirements.txt`no upper version limit (`>=0.40.0`without`<1.0.0`) | `requirements.txt`|
| Low |`buscar_na_web`returns generic mock data (intentional, but could be more realistic) |`agent.py:71`|
| Info | Step 6 of the ROUTE suggests JSON persistence, but base code uses dict in RAM |`ROTEIRO.md`|

### Recommendations
1. Add validation regex before the`eval()`: `re.match(r'^[\d\s\+\-\*/%\(\)\.]+$', expr)`2. Consider`ast.literal_eval()` ou `sympy`for safe math
3. Add basic logging (`logging.basicConfig`)

---

## PROJECT 2 — CLAUDE CODE (8.0/10)

### Strengths
- 3 professional skills:`fix-issue`, `review-pr`, `write-tests`— all with clear steps and edge case treatment
-`scripts/setup.sh` e `scripts/check-config.sh`robust with`set -e`, colors, validations
-`exemplos/mcp-config.json`with 8 documented MCP servers
-`settings.json`with allow/deny lists + PreToolUse/PostToolUse hooks
-`ROTEIRO.md`with 8 steps (6h total) — well-calibrated progression

### Problems Found

| Severity | Problem | Location |
|---|---|---|
| Media |`CLAUDE.md`example describes project`task-api`(FastAPI), not the Claude Code project itself — can be confusing |`CLAUDE.md`|
| Low |`exemplos/CLAUDE.md.minimal`may have an abrupt ending (56 lines) — check if it covers the minimum |`exemplos/CLAUDE.md.minimal`|
| Low |`settings.json`has structure`hooks.PreToolUse[].hooks[]`(redundant nesting, but it's the actual Claude Code format) |`.claude/settings.json`|
| Info | Scripts use`set -e`but no`set -u`(undefined variables) |`scripts/*.sh`|

### Recommendations
1. Add note on top of`CLAUDE.md`: "This is an EXAMPLE for FastAPI project — adapt it to your project"
2. Review`CLAUDE.md.minimal`— ensure it includes at least 3 complete sections
3. Add`set -u`in bash scripts

---

## PROJECT 3 — ANTIGRAVITY (7.5/10)

### Strengths
-`CONTEXT.md`rich and realistic (10+ sections, full stack, workflows)
-`workspace.json`well structured with MCP servers, workflows, permissions
-`scripts/setup-mcp.sh`with`set -euo pipefail`— exemplary good practices
-`COMO_EXECUTAR.txt`accessible to laypeople with simple language
-`exemplos/workflow-feature.md`realistic showing Plan→Execute→Verify

### Problems Found

| Severity | Problem | Location |
|---|---|---|
| High | Antigravity is a restricted beta product — commands like`brew install --cask antigravity`may not work |`index.html`, `ROTEIRO.md`|
| Media | URLs like`antigravity.dev` e `g.co/antigravity`unverifiable — can frustrate users | Multiple files |
| Low |`workspace.json`reference`ARCHITECTURE.md`that does not exist in the project |`workspace.json:7`|
| Info | Very specific Antigravity UX/CLI details are based on reference documentation — mark as "based on beta documentation" | Various |

### Recommendations
1. Add warning banner at top: "Antigravity is in restricted beta. This guide is based on available official documentation. Instructions may change."
2. Include a “Plan B” section with alternatives if access is not available
3. Create`ARCHITECTURE.md`placeholder or remove reference from`workspace.json`---

## PROJECT 4 — CODEX CLI (8.0/10)

### Strengths
-`AGENTS.md`extremely detailed (477 lines) — real Node.js/Express project with 10 sections
-`COMO_EXECUTAR.txt`excellent for laypeople (285 lines, 3 OS, troubleshooting 7 problems)
-`exemplos/github-actions.yml`Functional AI Code Review workflow with anti-duplication
-`scripts/run-headless.sh`for CI/CD automation — search API key from`.env`safely
- Claude Code vs Codex comparison table in HTML is different

### Problems Found

| Severity | Problem | Location |
|---|---|---|
| Media | Check if`npm install -g @openai/codex`currently works — Codex CLI was released in April 2025 and may have changes |`README.md`, `index.html`|
| Low |`.codex/config.toml`use model`o4-mini`— confirm that it is the correct model name in the current version |`.codex/config.toml:6`|
| Low | GitHub Actions workflow reference flag`--quiet`that may have changed between versions |`exemplos/github-actions.yml`|
| Info |`AGENTS.md`is for example project "TaskPro" — add note that it is a template for adaptation |`AGENTS.md`|

### Recommendations
1. Add release note: "Tested with Codex CLI v0.X — commands may change in future releases"
2. Include link to Codex CLI releases on GitHub
3. Tag model`o4-mini`as "current standard" with documented alternatives

---

## MENU AND PROMPTS (9.5/10)

###`projetos/index.html`— No critical issues
- 4 cards with correct information and functional links
- Accurate comparison table (tool/company/model/config/open-source)
- Well-defined personas section
- Responsive: 2x2 desktop grid, 1 mobile column
- Link to`prompts.html`in the footer and return section

###`projetos/prompts.html`— No critical issues
- 5 specialized prompts that would generate useful results in any AI
- "Copy" button with fallback (Clipboard API + execCommand)
- Clear differentiation between CLAUDE.md, AGENTS.md and CONTEXT.md
- "Tips for Better Results" section added

### Landing page (`index.html`) — No problem
- "Practical Projects" section correctly integrated with 4 cards
- Link to prompts.html present
- Visual consistent with the rest of the landing page

---

## CROSS-CUTTING PROBLEMS

| # | Problem | Projects | Action |
|---|---|---|---|
| 1 |`.gitignore`exists in the root of the repo but not within each project | All | Acceptable —`.gitignore`repo covers everything |
| 2 | ZIPs can become out of date when files are modified | All | Recreate ZIPs after each modification |
| 3 | No project has its own CI/CD (lint, automatic tests) | P1 most affected | Consider GitHub Actions for P1 |
| 4 | Light mode CSS has loopholes in classes like`.text-neutral-600`| Everyone's HTML | Minimal visual impact |

---

## FINAL VERDICT

**Overall quality: HIGH** — All 4 projects are ready for educational use. The content is substantial (16,000+ lines), the documentation is comprehensive, and the design is consistent.

**Priority actions:**
1. Add beta/release notice in P3 (Antigravity) and release note in P4 (Codex)
2. Strengthen validation of the`eval()`on P1
3. Add a note to P2's CLAUDE.md indicating that it is an adaptive example
4. Periodically check whether the P3 and P4 installation commands are still functional

---

*Report generated with Claude Opus 4.6 — 2026-03-03*
*Agentic Engineering Masterclass · duclub*