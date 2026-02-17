---
name: build-twfe-explainer
description: Build a comprehensive TWFE Mathematics Explainer using a multi-agent pipeline with empirical data integration
---

# TWFE Math Explainer — Multi-Agent Pipeline

Build a definitive, self-contained HTML teaching document explaining TWFE mathematics, causal inference formalism, and how fixed effects change significance. Uses 6 specialized agents across 5 phases.

## Arguments

- `all` — Run the full pipeline (default)
- `phase0` through `phase5` — Run a specific phase (assumes prior phases completed)
- `data-only` — Run only Phase 1 (data extraction)
- `drafts-only` — Run only Phase 2 (assumes Phase 1 done)
- `verify-only` — Run only Phase 3 (assumes Phases 1-2 done)
- `assemble-only` — Run only Phase 4 (assumes Phases 1-3 done)

## Target Document Structure (23 sections)

```
PRIMERS (P1–P7)
  P1: What Is a Regression?  (Gauss/Legendre history, full OLS derivation)
  P1½: Language of Statistics  (variance, covariance, key identities)
  P2: The Problem of Confounders  (Simpson's paradox)
  P3: Fixed Effects  (within estimator derivation)
  P4: The t-Statistic  (Student/Gosset 1908 history)
  P5: Panel Data & Pseudo-Panels
  P6: The Language of Causality  ← NEW (Potential Outcomes + DAGs)

ROAD MAP  (visual flow diagram)

CORE ANALYSIS (S1–S9)
  S1: Setup — The Specification Ladder  (empirical table)
  S2: FWL Theorem  (Frisch & Waugh 1933, Lovell 1963 — full dissertation)
  S3: SE Formula  (6-step derivation from first principles)
  S4: Variance Decomposition  (the key asymmetry)
  S5: Transition (1)→(2) — The Critical Jump
  S6: Transition (2)→(3) — State FE Refinement
  S7: Transition (3)→(4) — Cost of Over-Saturation
  S8: OVB  (full derivation + Simpson's paradox worked example)
  S9: Summary

ADVANCED TOPICS (S10–S16)
  S10: Identification & Parallel Trends  (potential outcomes + DAG + shift-share)
  S11: Pseudo-Panel  (Deaton 1985 — full history + derivation)
  S12: Moulton Problem  (Moulton 1990 — history + derivation)
  S13: Oster Test  (AET 2005 + Oster 2019 — full history)
  S14: Statistical Power & Honest Assessment
  S15: Geometry of FWL
  S16: Causal Graphs Applied  ← NEW (full DAG for this study)
```

## Workspace

All intermediate artifacts go in `results/.twfe_workspace/`. Create it at the start:

```bash
mkdir -p /Users/alessandro/Projects/Tesi/results/.twfe_workspace/drafts
```

---

## PHASE 0: SETUP

Create workspace and validate that required data files exist:

```bash
mkdir -p /Users/alessandro/Projects/Tesi/results/.twfe_workspace/drafts

# Verify data files exist
for f in \
  results/tables/twfe_main_results.csv \
  results/panels/panel_state_group.csv \
  results/panels/group_task_displacement_panel_delta.csv \
  results/intermediate/industry_task_displacement_ddelta.csv; do
  [ -f "/Users/alessandro/Projects/Tesi/$f" ] && echo "OK: $f" || echo "MISSING: $f"
done
```

If any files are missing, warn the user and suggest running the relevant analysis notebook first.

---

## PHASE 1: DATA EXTRACTION (blocking, 1 agent)

Launch ONE agent using the Task tool:

### Agent: twfe-data-extractor (sonnet)

```
subagent_type: general-purpose
model: sonnet
```

**Prompt**:
> You are the twfe-data-extractor agent. Your job is to extract empirical statistics from analysis files and produce a structured JSON.
>
> Read the agent instructions at `.claude/agents/twfe-data-extractor/twfe-data-extractor.md` for the complete specification.
>
> **Data files to read**:
> - `results/tables/twfe_main_results.csv` — regression results
> - `results/panels/panel_state_group.csv` — panel data for variance decomposition
> - `results/panels/group_task_displacement_panel_delta.csv` — national panel
> - `results/intermediate/industry_task_displacement_ddelta.csv` — industry-level TD
> - `analysis/02_cces_analysis/02_twfe_analysis.ipynb` — for any computed statistics
>
> Compute all statistics specified in the agent instructions and write the output to:
> `results/.twfe_workspace/empirical_data.json`
>
> Use Python via Bash for all computations. Every number must come from actual data, never hardcoded.
> Create the workspace directory if it doesn't exist.

**Wait for completion.** Verify the JSON is valid:

```bash
python3 -c "import json; json.load(open('results/.twfe_workspace/empirical_data.json')); print('JSON valid')"
```

---

## PHASE 2: DRAFTING (3 agents in parallel)

Launch THREE agents in parallel using the Task tool:

### Agent A: twfe-primer-drafter (opus) → primers.html

```
subagent_type: general-purpose
model: opus
```

**Prompt**:
> You are the twfe-primer-drafter agent. Write the Primers (P1-P7) and Roadmap for the TWFE Math Explainer.
>
> Read the agent instructions at `.claude/agents/twfe-primer-drafter/twfe-primer-drafter.md` for the complete specification. Follow them EXACTLY — they contain detailed requirements for each section.
>
> **Key inputs**:
> - `results/.twfe_workspace/empirical_data.json` — empirical numbers to embed
> - `results/twfe_math_explained.html` — existing document for CSS class reference
>
> **CRITICAL — EXHAUSTIVE THEORETICAL TREATMENT**:
> This is a DEFINITIVE teaching document, not a summary. Every primer must be written as a TEXTBOOK CHAPTER:
> - **P1 (Regression)**: Full 400-600 word historical narrative covering Legendre (1805), Gauss (1809), their priority dispute, Galton's "regression to the mean" (1886), Pearson's formalization, the Gauss-Markov theorem. Then complete OLS derivation from scratch — set up loss function, take derivatives, solve normal equations, get $\hat{\beta} = (X'X)^{-1}X'Y$. Show EVERY step.
> - **P3 (Fixed Effects)**: Full within-estimator derivation. Start from the individual model, compute group means, subtract, show equivalence to dummy variable regression via FWL. Discuss Mundlak (1978).
> - **P4 (t-Statistic)**: Tell the FULL Gosset/Guinness story (400+ words). Derive the t-distribution from the ratio of a normal and independent chi-squared.
> - **P6 (Causality — NEW, most important, 600-900 words)**: Three-part dissertation: (A) Potential Outcomes — Neyman 1923, Rubin 1974, Holland 1986, the Fundamental Problem, SUTVA, selection bias decomposition (DERIVE it). (B) DAGs — Pearl 2000, d-separation, three fundamental structures (chain, fork, collider), backdoor criterion (FORMALIZE it). (C) Connection to TWFE — FE as conditioning, parallel trends in potential outcomes notation.
> - Every derivation shows EVERY step. Never write "it can be shown" or "after some algebra."
> - Include analogy boxes, plain english boxes, worked examples, and connection notes for ALL primers
> - Use `$...$` and `$$...$$` for KaTeX math
> - All empirical numbers must come from empirical_data.json
>
> Write output to: `results/.twfe_workspace/drafts/primers.html`
> HTML body content only — no `<html>`, `<head>`, or `<style>` wrapper.

### Agent B: twfe-core-drafter (opus) → core.html

```
subagent_type: general-purpose
model: opus
```

**Prompt**:
> You are the twfe-core-drafter agent. Write Core Analysis sections S1-S9 for the TWFE Math Explainer.
>
> Read the agent instructions at `.claude/agents/twfe-core-drafter/twfe-core-drafter.md` for the complete specification. Follow them EXACTLY.
>
> **Key inputs**:
> - `results/.twfe_workspace/empirical_data.json` — empirical numbers to embed
> - `results/twfe_math_explained.html` — existing document for CSS class reference
>
> **CRITICAL — EXHAUSTIVE THEORETICAL TREATMENT**:
> This is a DEFINITIVE teaching reference. Never write "it can be shown" or "after some algebra." Show EVERY step.
> - **S2 (FWL — 400-600 lines)**: EXHAUSTIVE dissertation. Full history: Frisch's "confluence analysis" program, the 1933 Econometrica paper with Waugh, the agricultural price problem, Lovell's 1963 generalization for seasonal adjustment, why this is the most important theorem in applied econometrics. Then COMPLETE proof: define $M_2$, prove it's idempotent and symmetric, show $M_2 X_2 = 0$, premultiply the model, derive the equivalence with the full model. 8+ steps, every one shown.
> - **S3 (SE Formula — 350-500 lines)**: Not 6 steps but 10+. Start from $\hat{\beta} = (X'X)^{-1}X'Y$, derive the sampling error, prove the variance-of-linear-transformation rule, get the sandwich formula, specialize to homoskedastic case, then White (1980) robust case, then cluster-robust (Liang & Zeger 1986). Get to the scalar case $SE = \hat{\sigma}/\sqrt{SSX}$. Identify the THREE levers on SE (more data, more variation, less noise).
> - **S5/S6/S7 (transitions — 250-400 lines EACH)**: These are the CORE of the document. For each: use FWL to explain mechanically what happens, show empirical numbers in transition boxes, build worked examples with actual β/SE/t/R² values from empirical_data.json, and connect to OVB (the change in β between specs IS the OVB from the newly-added controls).
> - **S8 (OVB — 400-600 lines)**: EXHAUSTIVE dissertation. History from Fisher's confounder warnings through Simpson (1951) and Yule (1903) to Angrist & Pischke (2009). Full derivation from the partitioned model. Signed bias table. Then a COMPLETE worked example using Simpson's Paradox with this study's variables (education as confounder).
> - Use `$...$` and `$$...$$` for KaTeX math. All empirical numbers from empirical_data.json.
>
> Write output to: `results/.twfe_workspace/drafts/core.html`
> HTML body content only — no document wrapper.

### Agent C: twfe-advanced-drafter (opus) → advanced.html

```
subagent_type: general-purpose
model: opus
```

**Prompt**:
> You are the twfe-advanced-drafter agent. Write Advanced Topics sections S10-S16 for the TWFE Math Explainer.
>
> Read the agent instructions at `.claude/agents/twfe-advanced-drafter/twfe-advanced-drafter.md` for the complete specification. Follow them EXACTLY.
>
> **Key inputs**:
> - `results/.twfe_workspace/empirical_data.json` — empirical numbers to embed
> - `results/twfe_math_explained.html` — existing document for CSS class reference
>
> **CRITICAL — EXHAUSTIVE THEORETICAL TREATMENT**:
> Every section is a MINI-DISSERTATION on its topic. Tell the full human story behind every theorem. Derive every formula from first principles.
> - **S10 (Identification — 500-700 lines)**: EXHAUSTIVE. Full DiD history from Snow (1855) through Card & Krueger (1994) to the modern revolution (de Chaisemartin, Callaway & Sant'Anna). Parallel trends in potential outcomes notation (DERIVE the selection bias). 4 separate DAG diagrams (one per specification). Full Bartik/shift-share exposition: tell Bartik's story, explain BOTH the BHJ and GPSS interpretations, discuss which applies to our study and why.
> - **S11 (Pseudo-Panel — 350-500 lines)**: Full Deaton biography (Nobel 2015, Princeton, development economics context). Derive the aggregation, show the measurement error bias, prove it vanishes with large cells, show our actual cell sizes.
> - **S12 (Moulton — 350-500 lines)**: Full history from Moulton at BLS through Bertrand-Duflo-Mullainathan (2004). DERIVE the Moulton factor step by step from the equicorrelation structure — don't just state the formula. Show our actual cluster sizes and ICC.
> - **S13 (Oster — 350-500 lines)**: Full AET-to-Oster intellectual genealogy (Catholic schools debate → proportional selection → δ parameter). Derive Oster's closed-form δ formula. Apply to our specification ladder with actual R² values.
> - **S16 (Causal Graphs Applied — 400-600 lines)**: The PAYOFF section. Build 4 complete CSS-rendered DAGs showing what each specification blocks. Explain d-separation applied to our specific variables. Connect remaining open paths to Oster's δ.
> - Include `.dag-diagram` CSS class definition as an inline `<style>` block at the top of your output
> - Use `$...$` and `$$...$$` for KaTeX math. All empirical numbers from empirical_data.json.
>
> Write output to: `results/.twfe_workspace/drafts/advanced.html`
> HTML body content only, except for the dag-diagram `<style>` block.

**Wait for ALL 3 agents to complete before proceeding.**

---

## PHASE 3: VERIFICATION (1 agent)

Launch ONE agent:

### Agent: twfe-verifier (sonnet)

```
subagent_type: general-purpose
model: sonnet
```

**Prompt**:
> You are the twfe-verifier agent. Cross-check the drafted TWFE explainer sections for mathematical correctness, data accuracy, KaTeX syntax, CSS class usage, and HTML structure.
>
> Read the agent instructions at `.claude/agents/twfe-verifier/twfe-verifier.md` for the complete specification.
>
> **Files to verify**:
> - `results/.twfe_workspace/drafts/primers.html`
> - `results/.twfe_workspace/drafts/core.html`
> - `results/.twfe_workspace/drafts/advanced.html`
> - `results/.twfe_workspace/empirical_data.json` (source of truth for numbers)
>
> **Verify**:
> 1. Mathematical correctness of all derivations (OLS, FWL, SE, OVB, Moulton, Oster, potential outcomes)
> 2. Every empirical number matches empirical_data.json (to 3 significant figures)
> 3. KaTeX syntax is valid (even $ count, matched \begin/\end, proper braces)
> 4. All CSS classes used are defined
> 5. HTML structure is valid (matched tags, unique IDs)
> 6. All required components present (history, analogy, derivation, empirical application, plain english)
>
> Write report to: `results/.twfe_workspace/verification_report.md`

**Wait for completion.** Read the report summary:

```bash
head -20 results/.twfe_workspace/verification_report.md
```

If status is FAIL, review the errors. If critical math errors exist, consider re-running the affected drafter agent with corrections.

---

## PHASE 4: ASSEMBLY (1 agent)

Launch ONE agent:

### Agent: twfe-html-assembler (sonnet)

```
subagent_type: general-purpose
model: sonnet
```

**Prompt**:
> You are the twfe-html-assembler agent. Combine all drafted sections into a single, self-contained HTML document.
>
> Read the agent instructions at `.claude/agents/twfe-html-assembler/twfe-html-assembler.md` for the complete specification.
>
> **Files to read**:
> - `results/.twfe_workspace/drafts/primers.html` — Primers P1-P7 + Roadmap
> - `results/.twfe_workspace/drafts/core.html` — Core sections S1-S9
> - `results/.twfe_workspace/drafts/advanced.html` — Advanced sections S10-S16 (includes dag-diagram CSS)
> - `results/.twfe_workspace/verification_report.md` — Corrections to apply
> - `results/.twfe_workspace/empirical_data.json` — Source of truth for fixing number mismatches
> - `results/twfe_math_explained.html` — CSS template extraction
>
> **Steps**:
> 1. Extract full CSS from current twfe_math_explained.html
> 2. Add dag-diagram CSS from advanced.html's `<style>` block
> 3. Apply all corrections from verification report
> 4. Generate Table of Contents from section IDs
> 5. Assemble: header + TOC + primers + roadmap + hr + core + hr + advanced + footer
> 6. Validate: even $ count, matched section tags, all TOC hrefs resolve to section IDs
>
> Write final document to: `results/twfe_math_explained.html`
>
> The file must be completely self-contained (inline CSS, external only KaTeX CDN).
> If the file is very large, write it using Bash with a heredoc or Python script.

**Wait for completion.**

---

## PHASE 5: VALIDATION

After assembly, validate the output:

```bash
# File size and line count
wc -l results/twfe_math_explained.html
ls -lh results/twfe_math_explained.html

# Even $ count (KaTeX requirement)
DOLLAR_COUNT=$(grep -o '\$' results/twfe_math_explained.html | wc -l)
echo "Dollar signs: $DOLLAR_COUNT (must be even)"
[ $((DOLLAR_COUNT % 2)) -eq 0 ] && echo "PASS: Even $ count" || echo "FAIL: Odd $ count"

# Section tag matching
OPEN=$(grep -c '<section' results/twfe_math_explained.html)
CLOSE=$(grep -c '</section>' results/twfe_math_explained.html)
echo "Section tags: $OPEN open, $CLOSE close"
[ "$OPEN" -eq "$CLOSE" ] && echo "PASS: Matched" || echo "FAIL: Mismatched"

# Broken anchor links
python3 -c "
import re
html = open('results/twfe_math_explained.html').read()
hrefs = set(re.findall(r'href=\"#([^\"]+)\"', html))
ids = set(re.findall(r'id=\"([^\"]+)\"', html))
broken = hrefs - ids
if broken:
    print(f'FAIL: {len(broken)} broken links: {broken}')
else:
    print(f'PASS: All {len(hrefs)} anchor links resolve')
"

# Check for empty sections
python3 -c "
import re
html = open('results/twfe_math_explained.html').read()
sections = re.findall(r'<section[^>]*id=\"([^\"]+)\"[^>]*>(.*?)</section>', html, re.DOTALL)
for sid, content in sections:
    stripped = re.sub(r'<[^>]+>', '', content).strip()
    if len(stripped) < 100:
        print(f'WARNING: Section #{sid} has very little content ({len(stripped)} chars)')
print(f'Total sections: {len(sections)}')
"
```

Report results to user. If validation fails, diagnose and fix (or re-run the assembler).

Optionally open in browser:
```bash
open results/twfe_math_explained.html
```

---

## Cleanup

After successful validation, ask the user before cleaning up:

> The TWFE explainer has been built successfully. The workspace `results/.twfe_workspace/` contains intermediate artifacts (empirical_data.json, draft HTML files, verification report). Would you like to keep these for inspection, or shall I clean them up?

If user approves:
```bash
rm -rf /Users/alessandro/Projects/Tesi/results/.twfe_workspace
```

---

## Key Data Sources Reference

| File | Used By | What |
|------|---------|------|
| `results/tables/twfe_main_results.csv` | data-extractor | β, SE, t, p, N, R² for all specs |
| `results/panels/panel_state_group.csv` | data-extractor | Variance decompositions, FWL stats |
| `results/panels/group_task_displacement_panel_delta.csv` | data-extractor | National panel summary stats |
| `results/intermediate/industry_task_displacement_ddelta.csv` | data-extractor | Industry examples |
| `analysis/02_cces_analysis/02_twfe_analysis.ipynb` | data-extractor | Computed statistics |

## Historical Dissertations Tracker

| Concept | Section | Key Papers | Status |
|---------|---------|------------|--------|
| OLS / Least Squares | P1 | Legendre 1805, Gauss 1809 | Primers drafter |
| Student's t-test | P4 | Gosset 1908 | Primers drafter |
| Potential Outcomes | P6 | Neyman 1923, Rubin 1974, Holland 1986 | Primers drafter |
| DAGs | P6 | Pearl 2000, Spirtes et al. 1993 | Primers drafter |
| FWL Theorem | S2 | Frisch & Waugh 1933, Lovell 1963 | Core drafter |
| OVB Formula | S8 | Angrist & Pischke 2009 | Core drafter |
| Parallel Trends / DiD | S10 | Snow 1855, Card & Krueger 1994 | Advanced drafter |
| Shift-Share / Bartik | S10 | Bartik 1991, BHJ 2022, GPSS 2020 | Advanced drafter |
| Pseudo-Panel | S11 | Deaton 1985 | Advanced drafter |
| Moulton Problem | S12 | Moulton 1990 | Advanced drafter |
| Coefficient Stability | S13 | AET 2005, Oster 2019 | Advanced drafter |

## CSS Classes Available

All CSS is defined in the existing `twfe_math_explained.html`:

| Class | Purpose |
|-------|---------|
| `.primer-section` | Yellow-gradient section for primers |
| `.plain-english` | Green box with "Plain English" header |
| `.analogy-box` | Yellow dashed box with "Real-World Analogy" header |
| `.math-derivation` | Purple gradient box with "Derivation" header |
| `.worked-example` | Purple box with "Worked Example" header |
| `.transition-box` | Purple-bordered highlight box |
| `.eq` / `.eq.labeled` | Centered equation display |
| `.note-{blue,green,yellow,red,purple}` | Colored callout notes |
| `.metric-card` / `.grid-2` | Data display cards in 2-column grid |
| `.roadmap` / `.roadmap-flow` / `.roadmap-node` | Visual flow diagram |
| `.dag-diagram` | NEW — CSS causal graph (defined by advanced drafter) |

## Each Section Must Include — EXHAUSTIVE TREATMENT MANDATE

The document is a DEFINITIVE teaching reference — a self-contained textbook. Every theorem gets a full dissertation. Every formula is derived from first principles with every step shown. Never abbreviate, never hand-wave, never say "it can be shown that."

1. **Exhaustive historical/theoretical narrative** (400-800 words for major sections, 250-400 for others): The FULL intellectual story — who invented it, what problem they faced, what the climate was, what debates it sparked, who extended it. Names, dates, institutions, publication titles. The reader should understand the HUMAN story behind the mathematics.
2. **Analogy box**: Real-world analogy a high school student could follow
3. **Complete math derivation**: EVERY intermediate step visible. If a property (idempotency, Cauchy-Schwarz) is used, STATE and PROVE it inline. The reader must be able to verify every equation with pencil and paper.
4. **Intermediate theoretical results**: Prove lemmas, identities, and properties used in derivations — don't just cite them
5. **Empirical application**: Concrete numbers from `empirical_data.json` with interpretation
6. **Plain English box**: Summary without any mathematical notation
7. **Worked example**: Mandatory for major sections. Full numeric computation step by step.
8. **Cross-section connections**: Explicit forward/backward references linking sections into a coherent whole
