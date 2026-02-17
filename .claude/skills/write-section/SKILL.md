---
name: write-section
description: Write or revise any thesis chapter section with section-specific templates, style matching, and verification
---

# Write Section Skill

Write or revise a section of the thesis. This is a generalizable skill that adapts to any chapter based on section-specific templates and the established conventions from Chapters 3-5.

## Arguments

- `introduction` — Redirects to the `/write-introduction` skill (specialized)
- `literature [section]` — Write/revise a section of Ch.2 (e.g., `literature 2.1.4`)
- `abstract` — Write the thesis abstract (~250 words)
- `conclusion` — Write Ch.6 (Conclusion)
- `appendix A` or `appendix B` — Write appendix content
- `revise <chapter>` — Revise an existing chapter (e.g., `revise data`, `revise methodology`)

If argument is `introduction`, invoke the `/write-introduction` skill instead.

---

## Pre-Writing Protocol (ALL sections)

Before writing any section, you MUST:

1. **Read the target file** to understand current state (stub? outline? partial draft? complete?)
2. **Read `thesis/CLAUDE.md`** for conventions, bib keys, custom commands
3. **Read `CLAUDE.md` (root)** for project context, measure definitions, findings
4. **Read `.claude/reference/literature-evidence.md`** for concrete findings from 20+ papers — use for any section that references literature
5. **Read at least 40 lines** from `03_data.tex`, `04_methodology.tex`, and `05_results.tex` to internalize the writing style
6. **Read `references.bib`** to know which citations are available

### Using the Narrative Architect

For literature-heavy sections (Ch.2 subsections, Ch.1 Antecedents, Ch.6 Interpretation), dispatch the `narrative-architect` agent BEFORE drafting. Provide:
- **Goal**: What the section needs to establish
- **Target section**: e.g., "§2.2 — Economic Shocks and Political Behavior"
- **Constraints**: Length, must-cite papers, emphasis points

The architect returns a paragraph-by-paragraph outline with specific paper citations and transition strategies. Use this as the backbone for your draft.

### Style Requirements (from Chapters 3-5)

| Dimension | Convention |
|-----------|-----------|
| Person | First singular: "I define", "I estimate" |
| Tense | Present for methodology, past for data/literature |
| Sentences | 25-35 words average, clause-rich |
| Paragraphs | Topic sentence → support → transition |
| Asides | Em-dashes (`---`), not parentheses |
| Enumeration | Inline (i)/(ii)/(iii), NOT bullet points for arguments |
| Citations | `\citet{}` for subject, `\citep{}` for parenthetical |
| Cross-refs | `\Cref{}` start of sentence, `\cref{}` mid-sentence |
| Numbers | Exact figures inline, never rounded |
| Emphasis | `\emph{}` on first use, never `\textbf{}` in prose |
| Hedging | "consistent with", "suggesting", "indicating" |
| Forbidden | "interestingly", "it is worth noting", "we", rhetorical questions |

---

## Section-Specific Templates

### Abstract (~250 words)

```
Structure:
1. Context (1-2 sentences): The phenomenon and why it matters
2. Research question (1 sentence): Clear, precise
3. Method (2-3 sentences): Data, measure, estimation strategy
4. Findings (2-3 sentences): Main results with magnitudes
5. Contribution (1-2 sentences): What this adds to the literature
```

Rules:
- No citations in the abstract
- No forward references
- Must stand completely alone
- Use present tense throughout
- Every sentence must be informative (no filler)

### Literature Review Section (Ch.2)

```
Structure per section:
1. Opening sentence: What this section covers and why
2. Core papers (3-5 per subsection): Narrative synthesis, NOT paper-by-paper summary
3. Comparative passages: "While X does A, Y does B, I instead do C"
4. Forward reference: Bridge to this thesis's approach
5. Closing transition to next section
```

Rules:
- **Synthesize, don't summarize**: Group papers by finding/methodology, not chronologically
- **Position the thesis**: Every subsection must contain at least one passage comparing prior work to this thesis
- **Forward references**: Use `\Cref{ch:methodology}`, `\Cref{ch:data}`, etc.
- **Discursive comparison** (from style-paraphraser): "While \citet{frey2018robot} uses commuting zones, I exploit group-level variation..."
- **No orphan citations**: Every `\cite` key must exist in `references.bib`
- **Use the evidence base**: Consult `.claude/reference/literature-evidence.md` for concrete findings and magnitudes. Use Section 7 flow templates as starting points for the narrative-architect agent.

#### New Section: §2.1.4 — The Global Decline of the Labor Share

This section needs to be ADDED to Ch.2. Template:

```
Content:
- Karabarbounis & Neiman (2014): Global labor share decline, investment goods prices
- Autor et al. (2020): Superstar firms and labor share
- Barkai (2020): Declining labor share and rising profit share
- Connection to task displacement: labor share decline as MEASUREMENT strategy
- Forward ref to \Cref{subsec:industry_td}: "I exploit this relationship..."
```

This section bridges the labor economics literature to the measurement strategy in Ch.4. It explains WHY declining labor share captures automation exposure.

#### Expanded Section: §2.2.3 — Shift-Share Identification

Expand the existing methodological parallels section:

```
Additional content:
- Borusyak, Hull & Jaravel (2022): Shift-share as IV, conditions for valid inference
- Goldsmith-Pinkham, Sorkin & Swift (2020): Shares-based vs shifts-based identification
- Connection to THIS thesis's design: predetermined shares from ACS, industry-level shifts from BEA-KLEMS
- Forward ref to \Cref{sec:empirical}
```

#### Shortened Section: §2.4 — Contribution

After `/write-introduction` migrates the detailed contribution to Ch.1:

```
New structure (1-2 paragraphs only):
- Brief enumeration of 4 contributions
- "As articulated in \Cref{ch:introduction}, this thesis contributes..."
- No detailed exposition (that's now in Ch.1)
```

### Conclusion (Ch.6)

```
Structure:
1. Summary of findings (2-3 paragraphs)
   - Restate RQ, method, main result
   - Key magnitudes (β, lag structure)
   - Heterogeneous effects

2. Interpretation (2-3 paragraphs)
   - What the results mean for the backlash vs insecurity debate
   - Why automation → conservative (mechanism)
   - Connection to broader populism literature

3. Policy implications (1-2 paragraphs)
   - Just transitions, retraining programs
   - Political economy of automation policy

4. Limitations (1-2 paragraphs)
   - Ecological inference caveat
   - Pseudo-panel vs true panel
   - Oster δ = 0.04 (sensitivity to unobservables)
   - External validity (US only)

5. Future research (1 paragraph)
   - Individual-level panel data
   - European comparison
   - Mechanism decomposition
   - GenAI era extension
```

Rules:
- **No new information**: Everything must reference findings from Ch.5
- **Honest about limitations**: Don't oversell
- **Forward-looking**: End on future research, not caveats
- **Length**: ~4-6 pages, not longer

### Appendix A (Additional Tables)

```
Content:
- Full regression tables with all coefficients
- Alternative specifications mentioned in Ch.5
- Summary statistics by demographic group
- Industry-level task displacement values
```

Rules:
- Every table must have a `\label{tab:app_...}` and be referenced from the main text
- Use `threeparttable` with notes
- Match the formatting of Ch.5 tables exactly

### Appendix B (Additional Figures)

```
Content:
- Diagnostic plots (residuals, leverage, etc.)
- Additional descriptive figures
- Sensitivity analysis plots
```

Rules:
- Every figure must have a `\label{fig:app_...}` and be referenced from main text
- Include substantive figure notes
- Check that files exist in `results/figures/` before referencing

---

## Post-Writing Protocol (ALL sections)

### 1. Compile

```bash
cd /Users/alessandro/Projects/Tesi/thesis && tectonic main.tex
```

### 2. Cross-Reference Check

Verify all `\Cref` and `\cref` targets exist. Tectonic will show `Undefined reference` warnings.

### 3. Citation Check

Verify all `\citet` and `\citep` keys exist in `references.bib`. Tectonic will show `Undefined citation` warnings.

### 4. Consistency Check

- Notation matches the notation registry (`.claude/rules/notation-registry.md`)
- Sign convention: higher TD = more automation exposure
- Group definition: 18 groups (Edu x Gender x Age)
- Panel sizes: national 486, state 23,814

### 5. Style Self-Review

Re-read the draft against the style checklist above. If the prose doesn't match Chapters 3-5, revise or dispatch to the `style-paraphraser` agent.

---

## Multi-Pass Workflow

For any non-trivial section (>100 lines), follow this multi-pass approach:

```
Pass 1: RESEARCH
  - Read relevant chapters, papers, analysis outputs
  - Identify all citations needed
  - List all cross-references needed

Pass 2: OUTLINE
  - Write paragraph-level outline (1 sentence per paragraph)
  - Verify logical flow
  - Check that all required content is covered

Pass 3: DRAFT
  - Write full prose following section template
  - Insert all citations and cross-references
  - Mark uncertain numbers with % TODO

Pass 4: STYLE
  - Match voice/rhythm to Chapters 3-5
  - Fix sentence length, transitions, paragraph structure
  - Remove filler and passive voice

Pass 5: VERIFY
  - Compile with tectonic
  - Check all references and citations
  - Content verification against source material
```

For short sections (<100 lines), passes 2-3 can be combined.
