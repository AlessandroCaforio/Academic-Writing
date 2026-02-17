---
name: write-introduction
description: Write the thesis Introduction (Chapter 1) using a research-informed, multi-pass workflow grounded in academic writing best practices
---

# Write Introduction Skill

Write Chapter 1 (Introduction) for the thesis "Automation and Political Ideology: Task Displacement and the Conservative Shift Among Exposed Workers."

This skill implements a structured, multi-pass writing workflow distilled from leading academic writing guides and adapted for a Master's thesis.

## Arguments

- `draft` — Run the full pipeline: research, draft, style-match, verify (default)
- `outline` — Produce only a detailed outline with paragraph-level annotations
- `revise` — Revise an existing draft of Ch.1 (assumes `01_introduction.tex` already has content)

## Academic Writing Foundations

This skill is built on best practices from:

- **Keith Head** ("The Introduction Formula"): Hook → Question → Antecedents → Value-Added → Roadmap
- **Jesse Shapiro** ("Four Steps to an Applied Micro Paper"): Distinguish *aspirational* paragraphs ("I show that X causes Y") from *contractual* paragraphs ("Section 2 does A, Section 3 does B"). Aspirational paragraphs are the heart of the introduction; contractual paragraphs are its skeleton.
- **Claudia Goldin** ("Writing Tips"): Lead with your strongest result. The paper is persuasion, not a mystery novel. No generic section headings.
- **Marc Bellemare** ("How to Write Applied Papers"): In a thesis, the introduction is longer than in a journal article — it should be self-contained enough that a reader who skips everything else still understands the contribution.
- **Gallea's one-shot template**: Hook → Gap → Contribution → Results → Roadmap — compressed for a journal paper but expandable for a thesis.

### Key Principles

1. **No mystery novels** (Goldin): State the main finding in the first 2 paragraphs. The reader should know "automation exposure → conservative shift" before paragraph 3.
2. **Every sentence earns its place** (Shapiro): No filler. No "it is worth noting." No "interestingly."
3. **Aspirational before contractual** (Shapiro): Tell the reader *what you found* before telling them *how the chapter is organized*.
4. **Self-contained** (Bellemare): A busy committee member who reads only Ch.1 should still understand the research question, method, key findings, and contribution.
5. **Bridge, don't duplicate** (Head): The Antecedents section is a 3-4 paragraph map of the literature, NOT a mini literature review. Point to Ch.2 for depth.

---

## Target Structure (~450-550 lines, ~8-10 pages)

```
Chapter 1: Introduction
├── 1. Hook (2-3 paragraphs)
│   Opening with the contemporary phenomenon: populist/conservative surge
│   + economic disruption from automation. Ground in a specific fact or
│   striking observation. Then pivot to the scholarly puzzle.
│
├── 2. Research Question (1 paragraph)
│   Clear, precise statement: "I ask whether workers in occupations more
│   exposed to task-displacing automation shift toward conservative
│   political ideology."
│
├── 3. This Thesis: Approach & Findings (3-4 paragraphs)
│   [ASPIRATIONAL — Shapiro]
│   What I do: task displacement measure from BEA-KLEMS + ACS + O*NET,
│   IRT ideology from CCES, pseudo-panel 2006-2022, TWFE estimation.
│   What I find: β > 0 (automation → conservative shift), heterogeneous
│   effects, lag structure.
│   How I identify: shift-share logic, pre-determination of shares.
│
├── 4. Antecedents (3-4 paragraphs)
│   [Head's "Antecedents" — brief, not Ch.2-depth]
│   a. Task-based framework (ALM 2003) → automation measurement
│   b. Economic shocks → politics (ADH 2013, Colantone-Stanig, Frey et al.)
│   c. Cultural backlash vs economic insecurity debate
│   Each paragraph should end with what the prior work DOESN'T do that
│   this thesis addresses.
│
├── 5. Contribution (3-4 paragraphs)
│   [Moved from Ch.2 §2.4 — Head's "Value-Added"]
│   a. Comprehensive automation measure (task displacement, not just RTI/robots)
│   b. Continuous ideology outcome (IRT estimates, not binary vote choice)
│   c. Pseudo-panel spanning 2006-2022 (not single cross-section)
│   d. Mechanism adjudication (economic insecurity vs cultural backlash)
│   For each: why it matters, what it enables that prior work couldn't do.
│
├── 6. Roadmap (1 paragraph)
│   [CONTRACTUAL — Shapiro]
│   Chapter-by-chapter preview using \Cref{ch:...} for all chapters.
│   Brief, functional, no more than 1 sentence per chapter.
│
└── \Cref connections to all subsequent chapters
```

---

## PHASE 1: RESEARCH (read existing chapters)

Before writing a single word, you MUST read the following files to understand what the introduction needs to reference and not duplicate:

### Mandatory Reads

```
1. thesis/CLAUDE.md                                → conventions, commands, bib keys
2. .claude/reference/literature-evidence.md        → concrete findings from 20+ papers (use for Antecedents)
3. thesis/chapters/02_literature.tex               → what Ch.2 covers (avoid duplication)
4. thesis/chapters/03_data.tex                     → data sources, sample sizes, match rates
5. thesis/chapters/04_methodology.tex              → formulas, specifications, identification
6. thesis/chapters/05_results.tex                  → main coefficients, key tables/figures
7. thesis/references.bib                           → available citation keys
8. CLAUDE.md (root)                                → project context, measure pipeline, findings
```

### What to Extract

| Source | Extract for Introduction |
|--------|------------------------|
| Ch.3 | Data sources (CCES 2006-2022, ACS, BEA-KLEMS, O*NET), sample size, IRT ideology variable |
| Ch.4 | Task displacement formula (briefly), 18 demographic groups, pseudo-panel design, TWFE spec |
| Ch.5 | Main coefficient (β=0.291, p=0.06), lag structure (k=1 strongest), heterogeneous effects |
| Ch.2 §2.4 | Contribution text to migrate to Ch.1 (shorten §2.4, expand Ch.1 §5) |
| `references.bib` | Available citation keys — NEVER cite a paper not in the bib file |

---

## PHASE 1.5: BUILD ARGUMENTATIVE STRUCTURE

Before drafting, use the **narrative-architect** agent to design the argumentative flow for the Antecedents and Contribution sections. The agent reads `.claude/reference/literature-evidence.md` and produces paragraph-by-paragraph outlines with specific paper citations.

### How to Use

Dispatch the `narrative-architect` agent with:
- **Goal**: "Build the Antecedents section of the Introduction: briefly position the thesis in three literatures (task-based framework, economic shocks → politics, mechanisms) and identify what prior work does NOT do."
- **Target section**: "Chapter 1, §4 (Antecedents)"
- **Constraints**: "3-4 paragraphs max. Must cite ALM 2003, ADHM 2020, Colantone & Stanig 2018, and Gallego & Kurer 2022. Each paragraph must end with a gap statement."

The architect will return a structured outline. Use it as the backbone for Phase 2 drafting.

### Argumentative Flow Templates (from literature-evidence.md §7)

The evidence base includes pre-built flow templates for:
- **§7.1**: The Introduction Paradox (automation → rightward, not leftward)
- **§7.5**: The 4 Contributions structure

Use these as starting points. The narrative-architect agent refines them into paragraph-level outlines with specific citations.

---

## PHASE 2: DRAFT

### Writing Rules (MANDATORY)

1. **Voice**: First person singular. "I define", "I construct", "I estimate", "I find". Never "we" or "the author."
2. **Tense**: Present tense for methodology ("I define 18 groups"), past tense for data ("The CCES was administered..."), present for findings ("The results indicate...").
3. **Citations**: `\citet{key}` when author is subject, `\citep{key}` for parenthetical. NEVER write "Author (Year)" manually.
4. **Cross-references**: `\Cref{ch:data}` at sentence start, `\cref{ch:data}` mid-sentence. Use liberally — the introduction is a navigation hub.
5. **Numbers**: Cite exact figures inline ("517,736 employed individuals", "83.4\% match rate"). Never round when precise numbers are available.
6. **No fabrication**: Every coefficient, p-value, and sample size must come from Ch.5 or the analysis outputs. If a number is not in the existing chapters, leave a `% TODO: verify [number]` comment.
7. **Equations**: At most 1-2 display equations in the introduction (the TD formula and maybe the TWFE spec). Keep them simple; full derivations are in Ch.4.
8. **Hedging**: "consistent with", "suggesting", "indicating". Never "clearly proves" or "undoubtedly."
9. **Paragraph structure**: Topic sentence → 2-4 supporting sentences → transition sentence. Every paragraph.
10. **No em-dashes**: Do NOT use `---` in prose. Use commas, parentheses, colons, or semicolons for asides and elaborations. Match Chapters 3-5 style.

### Anti-Patterns (NEVER do these)

- Start with "In recent years..." or "Automation has become increasingly important..."
- Write more than 2 sentences of abstract methodology without grounding in a concrete example
- Use the word "interesting", "important", or "noteworthy" as filler
- Write a paragraph that is just a list of citations: "Author1 (Year) found X. Author2 (Year) found Y. Author3 (Year) found Z."
- Include a subsection structure — the introduction is ONE continuous flow
- Duplicate the literature review — keep Antecedents to 3-4 paragraphs max
- Use rhetorical questions (except very sparingly)
- Write the roadmap paragraph as anything other than the LAST paragraph
- Reference tables/figures from Ch.5 by number (they might renumber) — use `\Cref{tab:label}`

### Hook Strategies (choose one)

**A. The Paradox Hook**: "Despite decades of rising returns to education and skill-biased technological change favoring cognitive work, the political response to automation has defied economic models: workers facing displacement move *rightward*, not toward the redistributive left."

**B. The Fact Hook**: "Between 2006 and 2022, the most automation-exposed demographic groups in the United States experienced a [X]-point rightward shift in ideology..."

**C. The Event Hook**: Start with a specific moment (2016 election, a factory closure, a policy debate) that crystallizes the tension.

**D. The Literature Gap Hook**: "The China Shock literature established that trade-induced labor market disruption shapes political attitudes. But automation—a structural shock that is slower, broader, and harder to attribute—has received far less systematic attention."

### Opening Paragraph Template

```latex
[Hook: 2-3 sentences establishing the phenomenon]
[Pivot: 1 sentence framing the scholarly question]
I ask whether workers in occupations more exposed to task-displacing
automation shift toward conservative political ideology. Using a
pseudo-panel of [N] demographic groups tracked through the Cooperative
Congressional Election Study from 2006 to 2022, I find that a
one-standard-deviation increase in group-level task displacement is
associated with a [β]-point rightward shift in ideology, a pattern
consistent with the cultural backlash mechanism proposed by
\citet{colantone2021backlash} and \citet{manunta2025identity}.
```

---

## PHASE 3: STYLE MATCHING

After drafting, the text must be passed through a **style normalization pass** to match Chapters 3-5. Either:

1. **Self-check**: Re-read 40+ lines each from `03_data.tex`, `04_methodology.tex`, and `05_results.tex`. Compare sentence length, clause complexity, transition patterns. Revise the draft to match.
2. **Use the style-paraphraser agent**: If the draft feels "AI-generic" or doesn't match the thesis voice, dispatch to the `style-paraphraser` agent.

### Style Checklist

- [ ] Average sentence length 25-35 words (mix short declarative + longer complex)
- [ ] No em-dashes in prose (use commas, parentheses, colons, semicolons)
- [ ] Colons before definitions/lists
- [ ] (i)/(ii)/(iii) for inline enumeration, not bullet points
- [ ] No "we", no passive where "I" works
- [ ] Topic sentence opens every paragraph
- [ ] Transition sentence closes every paragraph
- [ ] No `\textbf{}` in running prose
- [ ] Exact numbers, not rounded
- [ ] `\emph{}` on first use of technical terms

---

## PHASE 4: VERIFICATION

### Compile

```bash
cd /Users/alessandro/Projects/Tesi/thesis && tectonic main.tex
```

### Check for:

- [ ] No compilation errors
- [ ] No undefined citations (`Undefined citation` warnings)
- [ ] No undefined references (`Undefined reference` warnings)
- [ ] All `\Cref` targets exist in Ch.2-5
- [ ] §2.4 contribution text has been shortened (not duplicated in both Ch.1 and Ch.2)

### Content Verification

- [ ] Research question stated clearly within first 2 paragraphs
- [ ] Main finding previewed within first 3 paragraphs (Goldin: no mystery novel)
- [ ] At least 4 forward references to subsequent chapters
- [ ] Contribution section has 3+ distinct points
- [ ] Roadmap mentions all 5 subsequent chapters
- [ ] No number appears without a source (Ch.3-5 or analysis outputs)
- [ ] Introduction is self-contained (readable without Ch.2-5)

---

## Coordination with Ch.2 Restructuring

When this skill is used, Ch.2 §2.4 (Contribution) should be shortened to a brief (1-2 paragraph) summary that says "this thesis contributes in four ways" and then points to Ch.1 for the full articulation. The detailed 150-line contribution section currently in Ch.2 should be migrated to Ch.1 §5.

After writing Ch.1, update Ch.2's opening paragraph to include a backward reference:
```latex
As outlined in \Cref{ch:introduction}, this thesis contributes to
three literatures. This chapter reviews each in turn.
```

---

## Example: Real Introduction Openings from Related Papers

### Anelli, Colantone & Stanig (2019) — "We Were The Robots"
Opens with the economic mechanism: "automation represents a source of structural change in the economy that generates aggregate gains but with winners and losers." Then pivots to political consequences through multiple channels (anti-incumbent, economic nationalism, authoritarian attitudes). Uses specific mechanisms, not generic statements.

### Kurer & Palier (2024) — "The AI Shock and Socio-Political Attitudes"
Opens with the broader context: "This paper evaluates the role of digital economic shocks in affecting Americans' social, cultural, and political beliefs." Connects to existing literature on economic backlash from outsourcing/trade, then identifies the gap (AI's complementing vs displacing effects). Clear bifurcation hypothesis stated upfront.

### Autor, Dorn, Hanson & Majlesi (2020) — "Importing Political Polarization"
Opens with the puzzle: import competition from China affected both labor markets AND politics. States the finding in the first paragraph: trade shock moved districts toward more extreme candidates. Methodology follows immediately.

### Pattern: All strong introductions state the finding within the first 3 paragraphs.
