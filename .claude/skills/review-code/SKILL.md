---
name: review-code
description: Review a Python analysis notebook for correctness, reproducibility, and formula consistency
context: fork
agent: code-reviewer
---

# Review Code

Review a Python analysis notebook or script. The argument should be the notebook path.

## Instructions

1. Read the specified notebook/file at the given path
2. Read `thesis/chapters/04_methodology.tex` for formula reference
3. Read `.claude/rules/python-conventions.md` for coding standards
4. Follow the code-reviewer agent instructions

### Example Usage

- `/review-code analysis/02_task_displacement/labor_share.ipynb`
- `/review-code analysis/04_cces_analysis/CCES_automation_analysis.ipynb`
- `/review-code src/utils.py`

If no argument is provided, review the most recently modified notebook in `analysis/`.
