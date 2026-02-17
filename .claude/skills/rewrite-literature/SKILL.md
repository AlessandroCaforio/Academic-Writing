---
name: rewrite-literature
description: Orchestrate a deep rewrite of Chapter 2 (Literature Review) using multi-agent pipeline with RAG-backed evidence
---

# Literature Review Rewrite Orchestrator

Rewrite Chapter 2 from scratch with deep extraction from ~20 cited papers. Uses a 5-phase pipeline with 7 specialized agents.

## Arguments

- `all` — Run the full 5-phase pipeline (default)
- `phase1` through `phase5` — Run a specific phase (assumes prior phases completed)
- `section N` — Draft only section 2.N (for iterating on one section)

## Workspace

All intermediate artifacts go in `thesis/chapters/.litreview_workspace/`. Create it at the start:

```bash
mkdir -p /Users/alessandro/Projects/Tesi/thesis/chapters/.litreview_workspace/drafts
```

## Target Structure (~400-500 lines)

```
Chapter 2: Literature Review
├── Intro paragraph (preview sections using \Cref)
├── 2.1 Automation and the Labor Market
│   ├── 2.1.1 The Task-Based Framework
│   ├── 2.1.2 Labor Market Polarization
│   ├── 2.1.3 Task Displacement and Rent Dissipation
│   └── 2.1.4 Bridge paragraph → politics
├── 2.2 Economic Shocks and Political Attitudes
│   ├── 2.2.1 The China Shock and Political Consequences
│   ├── 2.2.2 Automation and Voting Behavior
│   └── 2.2.3 Methodological Parallels (NEW)
├── 2.3 Mechanisms: From Economic Disruption to Political Change
│   ├── 2.3.1 The Economic Insecurity Thesis
│   ├── 2.3.2 The Cultural Backlash Thesis
│   ├── 2.3.3 Status Threat and the 2016 Election
│   ├── 2.3.4 Psychological Foundations (DEEPENED)
│   └── 2.3.5 Identity Threat as Integrating Framework (NEW)
└── 2.4 Contribution of This Thesis
```

---

## PHASE 1: RESEARCH (parallel)

Launch TWO agents in parallel using the Task tool:

### Agent 1: Extraction Assembler (sonnet, read-only)

```
subagent_type: general-purpose
model: sonnet
```

**Prompt**:
> You are assembling a structured evidence dossier for a thesis literature review rewrite.
>
> Read ALL files in `thesis/references/literature_analysis/paper_extractions/*.json` (12 files), the concept graph at `thesis/references/literature_analysis/concept_graph.json`, and the mechanism extraction at `thesis/references/literature_analysis/mechanism_extraction.json`.
>
> Produce TWO JSON files in `thesis/chapters/.litreview_workspace/`:
>
> **dossier.json** — Organize ALL extracted evidence by the target section structure:
> ```json
> {
>   "2.1.1_task_framework": {
>     "papers": ["autor_levy_murnane_2003"],
>     "key_quotes": [{"text": "...", "page": N, "bibkey": "autor2003skill"}],
>     "claims": [{"claim": "...", "evidence": "...", "strength": "strong"}],
>     "concept_edges": ["task_model → routine_manual", "task_model → substitution"],
>     "constructs": [{"name": "...", "definition": "..."}]
>   },
>   "2.1.2_polarization": { ... },
>   ...
> }
> ```
>
> **gaps.json** — Identify areas where extractions are thin and RAG queries would help:
> ```json
> {
>   "gaps": [
>     {
>       "section": "2.1.3",
>       "topic": "rent dissipation mechanism details",
>       "query": "rent dissipation automation wage premium loss",
>       "reason": "Extraction has claim but no detailed mechanism quote"
>     },
>     ...
>   ]
> }
> ```
>
> Map evidence to these sections:
> - 2.1.1: autor_levy_murnane_2003 (task framework, DOT, routine/nonroutine)
> - 2.1.2: autor_dorn_2013 (polarization, U-shape, commuting zones, shift-share)
> - 2.1.3: acemoglu_restrepo_2025 (task displacement, labor share, rent dissipation)
> - 2.2.1: autor_dorn_hanson_majlesi_2020, colantone_stanig_2018 (China shock politics)
> - 2.2.2: frey_berger_chen_2018, anelli_colantone_stanig_2019 (robots and voting)
> - 2.2.3: methodological parallels (shift-share across studies, pseudo-panel)
> - 2.3.1: economic insecurity evidence (Colantone & Stanig, Norris/Inglehart)
> - 2.3.2: inglehart_norris_2016, norris_inglehart_2018 (cultural backlash)
> - 2.3.3: mutz_2018, morgan_2018 (status threat debate)
> - 2.3.4: osborne_2023, stenner_2005 (psychological mechanisms, dual-process)
> - 2.3.5: manunta_2025, gidron_hall_2017 (identity threat integration)
> - 2.4: contribution positioning
>
> Include ALL verbatim quotes with page numbers. Be exhaustive — this is the evidence base for the entire chapter.

### Agent 2: RAG Deepener (sonnet, Bash access)

```
subagent_type: general-purpose
model: sonnet
```

**Prompt**:
> You supplement the literature dossier with additional evidence from the thesis RAG corpus (105 papers, 10,217 chunks in ChromaDB).
>
> First, if `thesis/chapters/.litreview_workspace/gaps.json` exists, read it and use those queries. If not yet available, use the default queries below.
>
> For each query, run:
> ```bash
> cd /Users/alessandro/Projects/Tesi/thesis/references/RAG && source venv/bin/activate && python /Users/alessandro/Projects/Tesi/.claude/skills/verify-claims/scripts/query_rag.py "QUERY" --top-k 5 --json
> ```
>
> Run 20-30 queries targeting these underdeveloped topics:
>
> **Section 2.1 (Automation & Labor)**:
> 1. "task is routine if can be accomplished by machines following explicit programmed rules"
> 2. "Dictionary of Occupational Titles DOT task measures"
> 3. "labor market polarization U-shape employment"
> 4. "commuting zone shift-share Bartik instrument"
> 5. "task displacement labor share decline automation"
> 6. "rent dissipation wage premium automation"
> 7. "between-group inequality automation 52 percent"
>
> **Section 2.2 (Shocks & Politics)**:
> 8. "China import shock political polarization electoral consequences"
> 9. "trade exposure radical right nationalist party Europe"
> 10. "economic nationalism protectionism import competition"
> 11. "robot exposure Trump presidential election swing states"
> 12. "counterfactual occupation assignment automation voting"
> 13. "robot exposure distinct from trade exposure"
>
> **Section 2.3 (Mechanisms)**:
> 14. "economic insecurity hypothesis pocketbook egotropic sociotropic"
> 15. "cultural backlash silent revolution post-materialist values"
> 16. "status threat group dominance racial resentment"
> 17. "Morgan critique Mutz 51 vote switchers fixed effects"
> 18. "dual-process model RWA SDO authoritarianism"
> 19. "authoritarian dynamic normative threat activation"
> 20. "identity threat mediates economic cultural populism"
> 21. "subjective social status populist right"
> 22. "dangerous world belief right-wing authoritarianism"
> 23. "competitive jungle social dominance orientation"
>
> **Section 2.4 (Contribution)**:
> 24. "pseudo-panel repeated cross-section Deaton synthetic cohort"
> 25. "IRT ideology estimate ideal point continuous measure"
>
> Save results to `thesis/chapters/.litreview_workspace/rag_supplements.json`:
> ```json
> {
>   "queries": [
>     {
>       "query": "...",
>       "target_section": "2.1.1",
>       "results": [
>         {"source": "Author (Year)", "page": N, "text": "...", "similarity": 0.85}
>       ]
>     }
>   ]
> }
> ```
>
> Prioritize results with similarity > 0.70 that contain verbatim-quotable text with specific numbers, definitions, or methodological details.

**Wait for both agents to complete before proceeding to Phase 2.**

---

## PHASE 2: ARCHITECTURE (single agent)

### Agent 3: Narrative Architect (opus)

```
subagent_type: general-purpose
model: opus
```

**Prompt**:
> You are designing the narrative architecture for a thesis literature review chapter. You have access to:
>
> 1. Evidence dossier: `thesis/chapters/.litreview_workspace/dossier.json`
> 2. RAG supplements: `thesis/chapters/.litreview_workspace/rag_supplements.json`
> 3. Current Ch.2: `thesis/chapters/02_literature.tex` (the version being replaced)
> 4. Literature synthesis: `thesis/references/literature_analysis/literature_synthesis.md`
> 5. Chapter openings for forward refs:
>    - `thesis/chapters/03_data.tex` (first 30 lines)
>    - `thesis/chapters/04_methodology.tex` (first 30 lines)
>    - `thesis/chapters/05_results.tex` (first 30 lines)
>
> Read all of these files. Then produce `thesis/chapters/.litreview_workspace/outline.md` with this structure:
>
> ```markdown
> # Chapter 2 Outline: Literature Review
>
> ## Introductory Paragraph
> - Preview all four sections using \Cref{sec:lit_automation}, etc.
> - ~8-10 lines
>
> ## Section 2.1: Automation and the Labor Market
> \label{sec:lit_automation}
> Estimated: ~100-120 lines
>
> ### 2.1.1 The Task-Based Framework \label{subsec:task_model}
> - ~25-30 lines
> - Topic sentence: [exact sentence]
> - Paper ordering: ALM 2003 only
> - Key quotes to include: [list with page numbers]
> - Concepts to define: routine vs nonroutine, DOT operationalization, substitution/complementarity
> - Forward ref: \Cref{ch:data} (O*NET as modern DOT successor)
> - Transition to 2.1.2: [sentence bridging to polarization]
>
> ### 2.1.2 Labor Market Polarization \label{subsec:polarization}
> [same structure...]
>
> [continue for ALL subsections...]
>
> ## Citation Plan
> | BibTeX key | Sections used | Role (primary/supporting) |
>
> ## Forward Reference Map
> | \Cref target | Section where introduced | Context |
>
> ## Estimated Total: ~420 lines
> ```
>
> **Critical requirements**:
> - Every subsection must have a specific topic sentence drafted
> - Every transition between subsections must be planned
> - Every direct quote must be assigned to a specific location with page citation
> - The outline must specify which concept graph edges inform each transition
> - Section 2.2.3 (Methodological Parallels) is NEW — design it to bridge empirics → your methodology
> - Section 2.3.5 (Identity Threat as Integrating Framework) is NEW — must synthesize the mechanisms debate
> - Section 2.4 must connect to ALL of Ch.3, Ch.4, Ch.5
>
> **DISCURSIVE POSITIONING requirement**:
> The literature review must read as a CONVERSATION with other researchers, not a catalog of findings.
> For each subsection, the outline must plan at least one "positioning passage" that follows this pattern
> (common in top economics journals like QJE, AER, JEPS):
>
> **Pattern A — "Closely related / distinct from"**: Name 2-3 closest papers → describe what they do
> → pivot with "distinct from" / "I depart from" / "unlike" → explain your approach.
> Example from Autor et al. (2024, QJE): "Our work is closely related to Webb (2020) and Kogan et al. (2021),
> who use NLP tools to identify innovations... Distinct from this literature, we develop a method to..."
>
> **Pattern B — "While X... I instead..."**: Compare a specific methodological choice by naming what others did
> and explaining your alternative.
> Example: "While Frey et al. (2018) aggregate robot exposure at the commuting-zone level, and
> Anelli et al. (2019) exploit individual-level variation through counterfactual occupation assignment,
> I adopt a group-level pseudo-panel..."
>
> **Pattern C — "Building on X, but extending..."**: Acknowledge what you inherit and add.
> Example: "Following the shift-share logic of Autor & Dorn (2013), I aggregate industry-level shocks
> to demographic groups using base-year employment weights—but unlike their fixed-baseline approach,
> I use rolling windows that capture recent automation dynamics."
>
> **Pattern D — "None of these studies... This is the gap"**: After reviewing 3-4 related papers,
> identify what is missing that your thesis addresses.
>
> The outline must explicitly mark WHERE each positioning passage goes, using tags like:
> `[POSITION: Pattern B — compare Frey CZ-level vs Anelli individual vs my group-level]`
>
> Key comparisons to plan (at minimum):
> - Automation measure: robot counts (A-R 2020) vs RTI proxies (Autor-Dorn) vs task displacement (A-R 2025) → yours
> - Unit of analysis: commuting zones (ADH, Frey) vs individual (Anelli) vs demographic groups (yours)
> - Political outcome: vote share (Frey, ADHM) vs party ID vs IRT ideology (yours)
> - Identification: county-level shift-share (Frey) vs individual counterfactual (Anelli) vs group-level TWFE (yours)
> - Geography: US only (Mutz, Frey, ADHM) vs Europe (Colantone-Stanig, Anelli) vs your US state-level
> - Temporal design: cross-section (Inglehart-Norris) vs panel (Mutz, Morgan) vs pseudo-panel (yours)
>
> Write in the voice of a Bocconi Master's student: precise, measured, first person singular.

**Wait for completion before Phase 3.**

---

## PHASE 3: DRAFTING (4 parallel agents)

Launch FOUR section-drafter agents in parallel using the Task tool. Each writes one `.tex` file.

### Agent 4a-4d: Section Drafters (opus, 4 instances)

For each section (2.1, 2.2, 2.3, 2.4), use:

```
subagent_type: general-purpose
model: opus
```

**Prompt template** (adjust section number, label, and line target):

> You are drafting Section 2.X of the Literature Review chapter for a Bocconi Master's thesis.
>
> Read these files:
> 1. Your section in the outline: `thesis/chapters/.litreview_workspace/outline.md` (Section 2.X only)
> 2. Evidence for your section from: `thesis/chapters/.litreview_workspace/dossier.json` (keys starting with "2.X.")
> 3. RAG supplements: `thesis/chapters/.litreview_workspace/rag_supplements.json` (entries targeting "2.X.*")
> 4. BibTeX file: `thesis/references.bib` (to verify all keys exist)
> 5. Notation registry: `.claude/rules/notation-registry.md`
> 6. Citation standards: `.claude/rules/literature-citations.md`
> 7. Forward reference requirements: `.claude/rules/forward-references.md`
>
> Write `thesis/chapters/.litreview_workspace/drafts/sec_2.X.tex` containing ONLY the LaTeX content for this section (no `\chapter`, no preamble).
>
> **Requirements**:
> - First person singular: "I review", "I examine", "I identify"
> - Present tense for methodology descriptions, past tense for data/findings
> - Use `\citet{key}` for author-as-subject, `\citep{key}` for parenthetical
> - Every direct quote in `` ``...'' `` must have `\citep[p.~N]{key}`
> - Include `\label{subsec:...}` for each subsection
> - Use `\Cref{target}` for forward references to Ch.3-5
> - Section separators: `% ===` for section header, `% ---` for subsections
> - Target: ~[80-120] lines for Section 2.X
> - End with a transition sentence to the next section (except 2.4)
>
> **DISCURSIVE STYLE — CRITICAL**:
> Write as a CONVERSATION with the literature, not a paper-by-paper summary. Follow the patterns
> used in top economics journals (QJE, AER, JEPS). Specifically:
>
> 1. **Compare and position**: When discussing related papers, group them by what they share and
>    where they differ. Example: "While \citet{frey2018robot} aggregate robot exposure at the
>    commuting-zone level, \citet{anelli2019robots} exploit individual-level variation through a
>    counterfactual occupation assignment. I adopt a different unit of analysis---demographic
>    groups defined by education, gender, and age---motivated by the pseudo-panel design of
>    \citet{deaton1985panel}."
>
> 2. **Build intellectual genealogy**: Show how ideas evolved across papers.
>    Example: "The task displacement measure I employ builds on \citeauthor{autor2003skill}'s
>    framework but operationalizes it through labor share changes rather than occupational task
>    proxies. This direct measurement approach, developed by \citet{acemoglu2025automation},
>    captures all forms of automation---not just industrial robots."
>
> 3. **Discuss methodology discursively**: Don't just state what a paper found. Explain HOW they
>    identified the effect and what that implies. Example: "\citet{mutz2018status} exploits
>    within-person variation in a two-wave panel, finding that status threat---not personal economic
>    hardship---predicts vote switching. \citet{morgan2018status} challenges this identification:
>    with only 51 vote switchers in the panel, the fixed-effects estimates lack statistical power."
>
> 4. **Use transition patterns that compare**: Instead of "Next, I review X", write
>    "Polarization research established that automation reshapes the labor market along predictable
>    lines, but it relied on occupational task intensity as a proxy. A more direct measurement
>    approach has recently become available."
>
> 5. **NEVER** write sequential summaries: "Paper A found X. Paper B found Y. Paper C found Z."
>    Instead, weave them into a narrative: "A growing body of evidence links automation to
>    right-wing politics. \citet{frey2018robot} show that US counties with greater robot exposure
>    swung toward Trump, an effect distinct from the China trade shock documented by
>    \citet{autor2020importing}. In Europe, \citet{anelli2019robots} confirm this pattern at the
>    individual level, using a counterfactual occupation assignment that avoids the endogeneity of
>    current job choice."
>
> **For Section 2.1**: Cover ALM task framework, Autor-Dorn polarization, A-R task displacement + rent dissipation. Bridge to politics. Include positioning passages comparing automation measures (robot counts vs RTI vs task displacement).
> **For Section 2.2**: Cover China shock (ADHM, Colantone-Stanig), robot voting (Frey, Anelli), and NEW methodological parallels subsection. Emphasize discursive comparison of identification strategies across papers.
> **For Section 2.3**: Cover economic insecurity, cultural backlash, status threat + Morgan debate, psychological mechanisms (dual-process), and NEW identity threat integration. Present the Mutz-Morgan debate as a real intellectual exchange, not two separate summaries.
> **For Section 2.4**: Contribution positioning — task displacement > robot exposure, IRT ideology, pseudo-panel, mechanism adjudication. This section should be ENTIRELY structured around "closely related / distinct from" comparisons with prior work. Forward refs to Ch.3, 4, 5.
>
> Do NOT verify BibTeX keys exist — just use the keys from the citation standards rule. If a key might be missing, add a `% TODO: verify bibkey` comment.

**Wait for all 4 agents to complete before Phase 3.5.**

---

## PHASE 3.5: STYLE PARAPHRASING (4 parallel agents)

The section drafters (Phase 3) produce content with correct structure, citations, and arguments---but each agent writes in its own voice. This phase normalizes all four sections to the author's actual writing style, as demonstrated in Chapters 3-5.

Launch FOUR style-paraphraser agents in parallel using the Task tool (one per section):

### Agent 4.5a-4.5d: Style Paraphraser (sonnet, 4 instances)

For each section (2.1, 2.2, 2.3, 2.4), use:

```
subagent_type: style-paraphraser
model: sonnet
```

**Prompt template** (adjust section number):

> You are the style-paraphraser agent. Your task is to rewrite Section 2.X of the literature review draft so that it matches the author's voice from Chapters 3-5.
>
> **Step 1 — Internalize the author's style.** Read 80 lines from each of:
> - `thesis/chapters/03_data.tex` (lines 1-80)
> - `thesis/chapters/04_methodology.tex` (lines 1-80)
> - `thesis/chapters/05_results.tex` (lines 1-80)
>
> **Step 2 — Read the draft.** Read: `thesis/chapters/.litreview_workspace/drafts/sec_2.X.tex`
>
> **Step 3 — Rewrite.** Transform the prose paragraph by paragraph:
> - Match the author's sentence rhythm (complex, clause-rich, 25-35 words, em-dash parentheticals)
> - Use first person singular active voice ("I review", "I examine", not "we" or passive)
> - Apply the author's transition patterns ("While X..., Y demonstrated that...")
> - Use inline enumeration: "(i)..., (ii)..., and (iii)..." for method steps
> - Use "Three patterns emerge. First,... Second,... Third,..." for findings
> - Hedge with "consistent with", "suggesting", "may reflect" (never "clearly", "obviously")
> - Introduce technical terms with `\emph{}` on first use
> - No `\textbf{}` in running prose, no footnotes, no rhetorical questions
>
> **Step 4 — Preserve invariants.** The rewrite MUST keep:
> - Every `\citet{}`, `\citep{}`, `\citeauthor{}` citation (same keys, same positions)
> - Every equation and its `\label{}`
> - Every `\Cref{}` and `\cref{}` forward/back reference
> - Every `\label{subsec:...}` and `\label{sec:...}`
> - The same argument structure (claim ordering, evidence flow)
> - All `` ``...'' `` direct quotes with their page citations
>
> **Step 5 — Write output.** Overwrite `thesis/chapters/.litreview_workspace/drafts/sec_2.X.tex` with the style-normalized version.
>
> Before writing, verify: same number of `\cite` commands in input vs output. If any are missing, fix before saving.

**Wait for all 4 agents to complete before Phase 4.**

---

## PHASE 4: VERIFICATION (parallel)

Launch TWO agents in parallel:

### Agent 5: Claim Verifier (sonnet)

```
subagent_type: general-purpose
model: sonnet
```

**Prompt**:
> You verify every factual claim in the new literature review draft.
>
> Read all draft sections:
> - `thesis/chapters/.litreview_workspace/drafts/sec_2.1.tex`
> - `thesis/chapters/.litreview_workspace/drafts/sec_2.2.tex`
> - `thesis/chapters/.litreview_workspace/drafts/sec_2.3.tex`
> - `thesis/chapters/.litreview_workspace/drafts/sec_2.4.tex`
>
> For each factual claim with a citation:
> 1. Check against paper extractions in `thesis/references/literature_analysis/paper_extractions/*.json`
> 2. For claims not covered by extractions, query RAG:
>    ```bash
>    cd /Users/alessandro/Projects/Tesi/thesis/references/RAG && source venv/bin/activate && python /Users/alessandro/Projects/Tesi/.claude/skills/verify-claims/scripts/query_rag.py "CLAIM" --top-k 3 --json
>    ```
>
> Produce `thesis/chapters/.litreview_workspace/verification_report.md`:
> ```markdown
> # Claim Verification Report
>
> ## Summary
> - Total claims: N
> - SUPPORTED: N (%)
> - PARTIALLY SUPPORTED: N (%)
> - UNSUPPORTED: N (%)
> - CONTRADICTED: N (%)
>
> ## Detailed Results
>
> ### Section 2.1
> | # | Claim | Citation | Status | Note |
> |---|-------|----------|--------|------|
> | 1 | "routine tasks..." | autor2003skill p.1283 | SUPPORTED | Exact quote verified |
> ...
>
> ## Corrections Needed
> [List any claims that need rewording, with suggested corrections]
> ```

### Agent 6: Forward Compatibility Checker (sonnet)

```
subagent_type: general-purpose
model: sonnet
```

**Prompt**:
> You check that the new literature review properly sets up all concepts used in Chapters 3-5.
>
> Read:
> 1. All draft sections in `thesis/chapters/.litreview_workspace/drafts/`
> 2. Full Chapter 3: `thesis/chapters/03_data.tex`
> 3. Full Chapter 4: `thesis/chapters/04_methodology.tex`
> 4. Full Chapter 5: `thesis/chapters/05_results.tex`
>
> Check:
> 1. **Every concept** first used in Ch.3-5 that should be introduced in Ch.2 IS introduced
> 2. **Every `\Cref` forward reference** in the drafts points to a label that exists in Ch.3-5
> 3. **Every BibTeX key** used in the drafts exists in `thesis/references.bib`
> 4. **Notation consistency**: any math symbols used in Ch.2 match the notation registry
>
> Produce `thesis/chapters/.litreview_workspace/compat_report.md`:
> ```markdown
> # Forward Compatibility Report
>
> ## Missing Concept Setups
> [Concepts used in Ch.3-5 but not introduced in Ch.2]
>
> ## Broken Forward References
> [Any \Cref targets that don't exist]
>
> ## Missing BibTeX Keys
> [Any keys used in drafts but not in references.bib]
>
> ## Notation Issues
> [Any symbol inconsistencies]
>
> ## Status: [PASS / ISSUES FOUND]
> ```

**Wait for both agents to complete before Phase 5.**

---

## PHASE 5: REFINEMENT (single agent)

### Agent 7: LaTeX Polisher (sonnet, write access)

```
subagent_type: general-purpose
model: sonnet
```

**Prompt**:
> You are the final assembler for the literature review chapter. You combine section drafts into a polished `02_literature.tex`.
>
> Read:
> 1. All section drafts in `thesis/chapters/.litreview_workspace/drafts/`
> 2. Verification report: `thesis/chapters/.litreview_workspace/verification_report.md`
> 3. Compatibility report: `thesis/chapters/.litreview_workspace/compat_report.md`
> 4. Current chapter header pattern: `thesis/chapters/02_literature.tex` (first 8 lines only — preserve the chapter/label format)
> 5. LaTeX conventions: `.claude/rules/latex-conventions.md`
> 6. Forward references: `.claude/rules/forward-references.md`
>
> Tasks:
> 1. **Apply corrections** from the verification report (fix any UNSUPPORTED or CONTRADICTED claims)
> 2. **Add missing forward refs** from the compatibility report
> 3. **Add missing BibTeX entries** — if keys are used in the draft but missing from `thesis/references.bib`, list them as `% TODO: add to references.bib` comments
> 4. **Write the intro paragraph** previewing all sections with `\Cref` references
> 5. **Assemble** all sections into a single file with consistent formatting:
>    - Chapter header with `% ===` separators
>    - Section headers with `% ===`, subsection with `% ---`
>    - Consistent spacing
> 6. **Write the final file** to `thesis/chapters/02_literature.tex`
>
> The file must:
> - Start with `% =====...` / `\chapter{Literature Review}` / `\label{ch:literature}`
> - Be ~400-500 lines total
> - Use ONLY `\citet`, `\citep`, `\citeauthor` for citations (never manual "Author (Year)")
> - Have all `\label` prefixes correct: `sec:`, `subsec:`
> - End with no trailing blank lines after the last `\section`

---

## POST-PIPELINE: Compilation Test

After Phase 5 completes, compile the thesis:

```bash
cd /Users/alessandro/Projects/Tesi/thesis && tectonic main.tex 2>&1
```

Check for:
- **Errors**: Must fix before declaring success
- **Undefined references**: List any broken `\Cref` targets
- **Undefined citations**: List any missing BibTeX keys → add them to `references.bib`

If there are undefined citations, add the missing entries to `thesis/references.bib` and recompile.

---

## Cleanup

After successful compilation, optionally remove the workspace:
```bash
rm -rf /Users/alessandro/Projects/Tesi/thesis/chapters/.litreview_workspace
```

Ask the user before cleaning up — they may want to inspect intermediate artifacts.

---

## BibTeX Key Reference

| Extraction name | BibTeX key |
|----------------|-----------|
| autor_levy_murnane_2003 | `autor2003skill` |
| autor_dorn_2013 | `autor2013polarization` |
| inglehart_norris_2016 | `inglehart2016trump` |
| colantone_stanig_2018 | `colantone2018trade` |
| mutz_2018 | `mutz2018status` |
| morgan_2018 | `morgan2018status` |
| frey_berger_chen_2018 | `frey2018robot` |
| norris_inglehart_2018 | `norris2019cultural` |
| anelli_colantone_stanig_2019 | `anelli2019robots` |
| autor_dorn_hanson_majlesi_2020 | `autor2020importing` |
| osborne_2023 | `osborne2023psychological` |
| acemoglu_restrepo_2025 | `acemoglu2025automation` |

Additional keys available in `references.bib`: `acemoglu2020robots`, `autor2013china`, `colantone2021backlash`, `colantone2019surge`, `manunta2025identity`, `stenner2005authoritarian`, `duckitt2009dual`, `duckitt2017dual`, `deaton1985panel`, `gidron2017politics`, `gest2018nostalgic`, `feldman1997perceived`, `stenner2018momentary`, `salmela2017emotional`, `jost2003political`, `ferwerda2025nostalgic`
