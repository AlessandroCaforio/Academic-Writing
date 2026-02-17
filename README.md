# Claude Code Thesis Workflow

> A multi-agent workflow built with [Claude Code](https://docs.anthropic.com/en/docs/claude-code) for writing an empirical economics Master's thesis — from data analysis to LaTeX production.

This repository documents the **complete AI-assisted workflow** I used to write my Master's thesis at Bocconi University, studying the effect of automation-driven task displacement on political ideology in the United States. The workflow leverages Claude Code's agent, rule, and skill system to create a specialized thesis-writing environment.

**This is not a template you install** — it's a real-world example of how to configure Claude Code for a complex, multi-month academic project. Feel free to adapt any part of it to your own work.

---

## What's Inside

```
.claude/
├── agents/          17 specialized sub-agents
├── rules/            8 project-wide rules (always loaded)
├── skills/          11 slash commands (user-invocable pipelines)
└── reference/        Literature evidence knowledge base

CLAUDE.md            The "project brain" — 600+ lines of persistent context
```

### The Three Layers

| Layer | What it does | How it works |
|-------|-------------|--------------|
| **CLAUDE.md** | Persistent project memory | Always loaded into context. Contains data sources, measure pipeline, code decisions, regression results, notation conventions, and project status. Acts as a single source of truth across sessions. |
| **Rules** (`.claude/rules/`) | Enforce standards automatically | Always-on constraints that Claude follows for every interaction. Notation consistency, citation format, LaTeX conventions, causal inference standards, etc. |
| **Skills** (`.claude/skills/`) | User-invocable workflows | Slash commands like `/proofread`, `/check-methodology`, `/verify-claims` that trigger multi-step pipelines. Some dispatch to sub-agents. |
| **Agents** (`.claude/agents/`) | Specialized sub-agents | Invoked by skills or manually. Each has domain expertise, specific tools, and focused instructions. E.g., the `causal-inference` agent runs on Opus and reviews TWFE identification strategy. |

---

## Agents (17)

### Thesis Writing
| Agent | Model | Purpose |
|-------|-------|---------|
| `thesis-writer` | default | Write LaTeX chapters matching the author's voice and conventions |
| `style-paraphraser` | default | Rewrite draft prose to match established writing style from existing chapters |
| `narrative-architect` | default | Build argumentative structure for sections, producing paragraph-level outlines |
| `proofreader` | default | Grammar, style, clarity, and academic tone review |

### Methodology & Verification
| Agent | Model | Purpose |
|-------|-------|---------|
| `causal-inference` | **opus** | Deep econometric review — TWFE validity, shift-share conditions, identification |
| `notation-checker` | default | Cross-chapter mathematical notation consistency |
| `latex-auditor` | default | Compilation issues, cross-references, citations, formatting |
| `verifier` | default | End-to-end verification — compiles thesis, checks regressions, validates requirements |

### Literature & Evidence
| Agent | Model | Purpose |
|-------|-------|---------|
| `literature-rag` | default | Verify claims against 105-paper ChromaDB corpus |
| `code-reviewer` | default | Review Python analysis notebooks for reproducibility and correctness |

### Geographic Visualization
| Agent | Model | Purpose |
|-------|-------|---------|
| `map-generator` | default | Create publication-quality CZ choropleth maps |

### TWFE Math Explainer (6 agents for one skill)
| Agent | Purpose |
|-------|---------|
| `twfe-primer-drafter` | Draft introductory primers (P1–P7) |
| `twfe-core-drafter` | Draft core analysis sections (S1–S9) |
| `twfe-advanced-drafter` | Draft advanced topics (S10–S16) |
| `twfe-data-extractor` | Extract empirical data from analysis files |
| `twfe-verifier` | Cross-check math, data accuracy, CSS, KaTeX |
| `twfe-html-assembler` | Combine all drafts into single valid HTML |

---

## Rules (8)

Rules are **always loaded** into Claude's context for every interaction in the project. They enforce consistency without manual prompting.

| Rule | Purpose |
|------|---------|
| `verification-protocol` | Compile with `tectonic` after every `.tex` edit |
| `quality-gates` | Advisory 1–10 scoring (clarity, rigor, consistency, completeness, presentation) — never blocking |
| `notation-registry` | Canonical symbol table with subscript ordering convention (g, s, i, t, k) |
| `latex-conventions` | Cross-references (`\Cref`), `booktabs` tables, `siunitx` columns, first-person style |
| `literature-citations` | `\citet` vs `\citep`, page numbers on quotes, BibTeX key reference table |
| `forward-references` | Required forward `\Cref` links from Ch.2 to Chapters 3–5 |
| `python-conventions` | `pathlib`, person weights, sign conventions, formula-code consistency |
| `causal-inference` | TWFE requirements, shift-share design, pseudo-panel methodology, inference standards |

---

## Skills (11)

Skills are invoked with slash commands (e.g., `/proofread`) and run multi-step workflows.

### Core Writing
| Skill | Command | What it does |
|-------|---------|--------------|
| Write Section | `/write-section` | Write or revise any thesis chapter section with style matching |
| Write Introduction | `/write-introduction` | Write Chapter 1 using a structured pipeline |
| Rewrite Literature | `/rewrite-literature` | 5-phase orchestrated deep rewrite of Chapter 2, using 7 sub-agents |

### Quality Assurance
| Skill | Command | What it does |
|-------|---------|--------------|
| Proofread | `/proofread` | Grammar, style, clarity review of a chapter |
| Check Methodology | `/check-methodology` | Deep econometric review (dispatches to `causal-inference` agent on Opus) |
| Thesis Excellence | `/thesis-excellence` | Multi-agent review dispatching to specialized agents |
| Verify Claims | `/verify-claims` | Check thesis claims against 105-paper RAG corpus |

### Utilities
| Skill | Command | What it does |
|-------|---------|--------------|
| Compile Thesis | `/compile-thesis` | Run `tectonic main.tex` and report errors |
| Review Code | `/review-code` | Review Python notebooks for correctness and consistency |
| Generate Maps | `/generate-maps` | Regenerate all CZ choropleth map figures |
| Build TWFE Explainer | `/build-twfe-explainer` | 6-agent pipeline to build an interactive HTML math explainer |

---

## The Project Brain (`CLAUDE.md`)

The `CLAUDE.md` file is the most important piece. At ~600 lines, it acts as **persistent memory** across all Claude Code sessions. It contains:

1. **Project structure** — directory layout with descriptions of every file
2. **Data sources** — paths, formats, quirks, matching rates (e.g., "5-digit broad SOC for O*NET matching gives 83.4% match rate")
3. **Measure pipeline** — step-by-step construction from BEA-KLEMS → ACS weights → group aggregation → CCES panel → TWFE
4. **Code decisions** — why specific choices were made (task displacement vs. RTI, sign conventions, pseudo-panel approach)
5. **Key findings** — regression coefficients, standard errors, significance levels
6. **DO / DON'T lists** — explicit guardrails (e.g., "DO NOT suggest td_adjusted as primary measure")
7. **Session log** — chronological record of major decisions

This means every new Claude Code session starts with **full context** about the project — no re-explaining needed.

---

## RAG Integration

The `/verify-claims` skill connects to a local ChromaDB vector database containing:
- **105 indexed papers** (full text, chunked)
- **10,217 chunks** with author, title, and page metadata
- **75-node concept graph** (105 edges) linking papers to theoretical concepts
- **Embedding model**: `all-MiniLM-L6-v2`

The bridge script (`.claude/skills/verify-claims/scripts/query_rag.py`) lets Claude search the literature corpus and verify specific claims with page-level citations.

---

## Architecture Diagram

```
User
 │
 ├── /proofread ──────────► proofreader agent
 ├── /check-methodology ──► causal-inference agent (Opus)
 ├── /verify-claims ──────► literature-rag agent ──► ChromaDB (105 papers)
 ├── /write-section ──────► thesis-writer agent
 ├── /write-introduction ─► narrative-architect ──► thesis-writer
 ├── /rewrite-literature ─► 5-phase pipeline (7 agents)
 ├── /thesis-excellence ──► dispatches to multiple agents
 ├── /generate-maps ──────► map-generator agent
 ├── /build-twfe-explainer► 6-agent pipeline (primers → core → advanced → verify → assemble)
 ├── /compile-thesis ─────► tectonic (direct)
 └── /review-code ────────► code-reviewer agent

 Always active:
   ├── CLAUDE.md (project brain — full context every session)
   └── 8 Rules (notation, citations, LaTeX, causal inference, etc.)
```

---

## How I Built This (Lessons Learned)

### 1. Start with CLAUDE.md, add rules/agents later
The project brain came first. I began by documenting data sources, pipeline decisions, and findings. Rules and agents were added incrementally as pain points emerged (e.g., notation inconsistencies → `notation-registry` rule).

### 2. Rules for "always-on" constraints, skills for "on-demand" workflows
If something should be enforced in *every* interaction (notation, citation format), make it a rule. If it's a workflow you invoke occasionally (proofread a chapter), make it a skill.

### 3. The Opus escalation pattern
Most agents run on the default model (Sonnet), which is fast and cheap. The `causal-inference` agent runs on **Opus** because econometric methodology review requires deeper reasoning. This keeps costs low for routine tasks while preserving quality where it matters.

### 4. DO/DON'T lists prevent drift
After the first month, I noticed Claude occasionally suggesting approaches I'd already ruled out (e.g., `td_adjusted` as primary measure, foreign labor share as IV). Adding explicit "DO NOT suggest" lists in CLAUDE.md eliminated this.

### 5. Quality gates should never block
Early versions had quality scores that could prevent actions. This was frustrating — sometimes you need to write a rough draft. Making scores advisory-only (informational feedback, never blocking) was the right call.

### 6. The session log is underrated
The chronological log at the bottom of CLAUDE.md ("Session Log") seems simple, but it's crucial. When you return after a week, it tells you exactly where you left off and what decisions were made.

---

## Thesis Context

**Title**: Automation and Political Ideology: Task Displacement Effects on Ideological Positioning in the United States

**Research Design**:
- Shift-share (Bartik) identification: industry-level automation shocks × base-year employment composition
- Two-Way Fixed Effects (TWFE) on a pseudo-panel of 18 demographic groups × 9 election cycles (2006–2022)
- Primary outcome: IRT ideology scores (continuous); secondary: Republican vote share
- Geographic extensions at the state level (882 state×group cells) and commuting zone level (722 CZs)

**Key Finding**: A one-standard-deviation increase in task displacement is associated with a statistically significant conservative shift in ideology (β = 0.468, p = 0.020 for the preferred specification).

---

## How to Adapt This for Your Project

1. **Fork this repo** and strip out the thesis-specific content
2. **Start with CLAUDE.md** — document your data, pipeline, and key decisions
3. **Add rules** for conventions you want enforced everywhere (formatting, coding style, etc.)
4. **Create skills** for workflows you repeat (compile, test, review, deploy)
5. **Build agents** for tasks requiring specialized knowledge (domain expert, code reviewer, etc.)
6. **Iterate** — the best configurations emerge from actual pain points, not upfront planning

---

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI
- For the RAG integration: Python 3.10+, ChromaDB, sentence-transformers
- For LaTeX compilation: [tectonic](https://tectonic-typesetting.github.io/)

---

## License

MIT — use and adapt freely.

---

## About

Built by Alessandro Caforio during a Master's in Public Policy Analysis at Bocconi University (2025–2026), supervised by Professor Massimo Anelli.
