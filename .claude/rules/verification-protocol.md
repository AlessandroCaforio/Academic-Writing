---
globs: thesis/**/*.tex
---

# Verification Protocol for LaTeX Edits

After any edit to `.tex` files in the `thesis/` directory, you MUST compile the thesis to verify no errors were introduced.

## Compilation Command

```bash
cd /Users/alessandro/Projects/Tesi/thesis && tectonic main.tex
```

- **Tool**: `tectonic` (NOT pdflatex/MacTeX — they are not installed)
- Tectonic handles multiple passes and BibTeX automatically in one command
- Check the output for errors and warnings
- Report any `Undefined control sequence`, `Missing reference`, or `Undefined citation` warnings

## When to Compile

- After creating or editing any `.tex` file
- After modifying `references.bib`
- Before declaring a writing task complete

## Warning Triage

- **Errors** (compilation fails): Must fix before proceeding
- **Undefined references/citations**: Flag to user, likely needs another pass or missing label
- **Overfull/underfull hbox**: Informational only, do not fix unless user requests
