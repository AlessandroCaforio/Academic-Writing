---
name: proofread
description: Proofread a thesis chapter for grammar, style, and clarity
context: fork
agent: proofreader
---

# Proofread Chapter

Proofread the specified thesis chapter. The argument should be a chapter filename (e.g., `05_results` or `04_methodology.tex`).

## Instructions

1. Read the chapter file at `thesis/chapters/$ARGUMENTS.tex` (add `.tex` if not provided)
2. Follow the proofreader agent instructions to review grammar, style, clarity, and transitions
3. Return a structured report with findings sorted by severity (HIGH, MEDIUM, LOW)

If no argument is provided, ask which chapter to proofread.
