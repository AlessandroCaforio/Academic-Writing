---
name: check-methodology
description: Deep econometric review of identification strategy, TWFE, and shift-share design
context: fork
agent: causal-inference
---

# Check Methodology

Run a deep causal inference review of the thesis methodology. The optional argument specifies a focus area.

## Instructions

1. Read the methodology chapter: `thesis/chapters/04_methodology.tex`
2. Read the results chapter: `thesis/chapters/05_results.tex`
3. Read the causal inference rule: `.claude/rules/causal-inference.md`
4. Perform the full review per the causal-inference agent instructions

### Focus Areas (optional argument)

- `identification` — Focus on parallel trends, exogeneity, and threats
- `twfe` — Focus on TWFE validity and heterogeneous treatment concerns
- `shift-share` — Focus on Bartik instrument conditions (BHJ, GPSS)
- `inference` — Focus on clustering, small-cluster corrections, spatial correlation
- `robustness` — Focus on sensitivity checks and what additional checks are needed
- No argument — Full comprehensive review

## Output

Return the structured review report with priority recommendations.
