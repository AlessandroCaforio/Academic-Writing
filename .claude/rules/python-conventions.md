---
globs: analysis/**/*.ipynb, src/**/*.py
---

# Python Conventions for Analysis Code

## Paths
- Use `pathlib.Path` for all file paths, never string concatenation
- Define `PROJECT_ROOT` and `RESULTS_DIR` at the top of each notebook

## Data Handling
- Always apply `PERWT` (person weight) when aggregating ACS data
- Use vectorized pandas/numpy operations — avoid `.append()` in loops
- For large files, use chunked reading: `pd.read_csv(..., chunksize=500_000)`
- Call `gc.collect()` after processing large dataframes

## Statistical Conventions
- Sign convention: higher TD = more automation exposure (sign flipped from raw)
- Subscript ordering in variable names: group, state, year, lag (g, s, t, k)
- Clusters: group-level for national, state-group for state specifications

## Reproducibility
- Set random seeds where applicable: `np.random.seed(42)`
- Pin library versions in `requirements.txt`
- Clear all outputs before committing notebooks (or use `--clear-output`)

## Formulas
- Any formula in code must match Chapter 4 exactly
- Key equations: omega (eq:omega), RCA (eq:rca), group TD (eq:group_td), state TD (eq:state_td)
- If a discrepancy is found between code and thesis, flag it immediately
