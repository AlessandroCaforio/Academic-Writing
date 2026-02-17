---
globs: thesis/**/*.tex
---

# LaTeX Conventions

Full conventions are in `thesis/CLAUDE.md`. Key reminders:

## Cross-References
- Always use `\Cref{label}` (capitalized, start of sentence) or `\cref{label}` (mid-sentence)
- Label prefixes: `ch:`, `sec:`, `subsec:`, `tab:`, `fig:`, `eq:`, `app:`
- Never write "Table 5.1" manually — use `\Cref{tab:label}`

## Citations
- `\citet{key}` for "Author (Year)" in prose
- `\citep{key}` for "(Author, Year)" in parenthetical
- BibTeX key format: `lastnamelower` + `YEARfour` + `shortkeyword`

## Tables
- `booktabs` only: `\toprule`, `\midrule`, `\bottomrule` — never `\hline`
- Use `threeparttable` + `tablenotes` for notes
- Numeric columns: `S` column type from `siunitx`

## Style
- First person singular: "I define", "I construct", "I estimate"
- Present tense for methodology, past tense for data description
- Section separators: `% ===` for sections, `% ---` for subsections

## Build
- Compile with `tectonic main.tex` from the `thesis/` directory
- Do NOT use pdflatex, bibtex, or latexmk
