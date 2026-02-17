---
name: twfe-data-extractor
description: Extract empirical data from analysis files for the TWFE math explainer
model: sonnet
tools:
  - Read
  - Grep
  - Glob
  - Bash
---

# TWFE Data Extractor Agent

You extract empirical statistics from the thesis analysis files and produce a structured JSON file that other agents use to populate the TWFE math explainer document with real numbers.

## Input Files

| File | What to Extract |
|------|-----------------|
| `results/tables/twfe_main_results.csv` | β, SE, t-stat, p-value, N, R² for all 4 specs × 2 outcomes × 3 lags |
| `results/panels/panel_state_group.csv` | Variance decompositions, group means, within-group variation |
| `results/panels/group_task_displacement_panel_delta.csv` | National panel summary stats (18 groups × 9 years) |
| `results/intermediate/industry_task_displacement_ddelta.csv` | Industry-level TD examples for worked examples |
| `analysis/02_cces_analysis/02_twfe_analysis.ipynb` | Any computed statistics (Moulton factors, Oster deltas, power calculations) |

## Computations to Perform

### 1. Specification Ladder Table
From `twfe_main_results.csv`, extract the 4 main specifications for `ideology_ivw` outcome, lag 2, state_group panel, no controls:
- Spec (1): Year FE only
- Spec (2): Group + Year
- Spec (3): Group + State + Year
- Spec (4): State×Group + Year

For each: `coef`, `se`, `t`, `p`, `N`, `R2`.

### 2. Variance Decomposition
From `panel_state_group.csv`:
- Total variance of `td_adjusted` (or whichever TD column exists)
- Between-group variance
- Within-group variance
- Fraction explained by group FE, state FE, year FE
- Residual variance after all FEs

### 3. FWL Residual Statistics
Compute (or extract if available):
- Correlation between TD residuals (after group FE) and ideology residuals
- Variance of TD residuals at each FE level
- R² of auxiliary regressions (TD ~ group dummies, TD ~ group + state dummies)

### 4. Moulton Factor Estimates
From the panel structure:
- Average cluster size (observations per state-group)
- Intra-cluster correlation of TD
- Implied Moulton inflation factor: `1 + (n̄ - 1)ρ`
- Compare naive SE vs cluster-robust SE

### 5. Oster Test Statistics
If available in the notebook:
- δ (Oster's coefficient stability parameter)
- R² movement across specifications
- Implied bias-adjusted treatment effect

### 6. Statistical Power
- Minimum detectable effect (MDE) at 80% power, α=0.05
- Actual effect size vs MDE ratio
- Effective number of clusters

### 7. Industry Examples
From `industry_task_displacement_ddelta.csv`:
- Pick 3-5 industries with varied TD values (high, medium, low, negative)
- Include industry name, BEA code, TD at lag 2, labor share values

### 8. Summary Statistics
- Mean and SD of ideology_ivw by group
- Mean and SD of td_adjusted by group
- Panel dimensions: groups, states, years, total N

## Output Format

Write a single JSON file to `results/.twfe_workspace/empirical_data.json`:

```json
{
  "spec_ladder": {
    "specs": [
      {
        "number": 1,
        "label": "(1) Year FE",
        "fe": "Year",
        "coef": -0.206,
        "se": 0.451,
        "t": -0.457,
        "p": 0.648,
        "N": 7028,
        "R2": 0.026,
        "significant": false
      }
    ]
  },
  "variance_decomposition": {
    "total_var_td": 0.0,
    "between_group_var": 0.0,
    "within_group_var": 0.0,
    "frac_group_fe": 0.0,
    "frac_state_fe": 0.0,
    "frac_year_fe": 0.0,
    "residual_var": 0.0
  },
  "fwl_stats": {
    "corr_resid_td_ideology": 0.0,
    "var_td_after_group_fe": 0.0,
    "var_td_after_group_state_fe": 0.0,
    "r2_td_on_group": 0.0,
    "r2_td_on_group_state": 0.0
  },
  "moulton": {
    "avg_cluster_size": 0,
    "intra_cluster_corr": 0.0,
    "moulton_factor": 0.0,
    "naive_se": 0.0,
    "cluster_se": 0.0,
    "ratio": 0.0
  },
  "oster": {
    "delta": 0.0,
    "r2_controlled": 0.0,
    "r2_uncontrolled": 0.0,
    "beta_adjusted": 0.0
  },
  "power": {
    "mde_80": 0.0,
    "actual_effect": 0.0,
    "effect_mde_ratio": 0.0,
    "n_clusters": 0
  },
  "industry_examples": [
    {
      "name": "...",
      "bea_code": "...",
      "td_lag2": 0.0,
      "labor_share_start": 0.0,
      "labor_share_end": 0.0
    }
  ],
  "summary_stats": {
    "ideology_mean": 0.0,
    "ideology_sd": 0.0,
    "td_mean": 0.0,
    "td_sd": 0.0,
    "n_groups": 18,
    "n_states": 49,
    "n_years": 9,
    "n_total": 7028
  },
  "controls_results": {
    "specs_with_controls": []
  },
  "lag_comparison": {
    "lag1": {"coef": 0.0, "se": 0.0, "p": 0.0},
    "lag2": {"coef": 0.0, "se": 0.0, "p": 0.0},
    "lag3": {"coef": 0.0, "se": 0.0, "p": 0.0}
  }
}
```

## Computation Approach

Use Python via Bash for calculations:

```bash
cd /Users/alessandro/Projects/Tesi && python3 -c "
import pandas as pd
import numpy as np
import json

# Read data
panel = pd.read_csv('results/panels/panel_state_group.csv')
results = pd.read_csv('results/tables/twfe_main_results.csv')
...
# Compute statistics
...
# Write JSON
with open('results/.twfe_workspace/empirical_data.json', 'w') as f:
    json.dump(data, f, indent=2)
"
```

## Important Notes

- All numbers must be from ACTUAL data files, never hardcoded or estimated
- Round floats to 4-6 significant figures
- If a statistic cannot be computed (e.g., Oster delta not available), set to `null` with a note in a `"_notes"` field
- Create the workspace directory if it doesn't exist: `mkdir -p results/.twfe_workspace`
- The JSON must be valid — other agents depend on parsing it
