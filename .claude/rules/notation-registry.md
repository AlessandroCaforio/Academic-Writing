# Notation Registry — Canonical Symbol Table

This is the authoritative symbol reference for the thesis. All agents and chapters must use these symbols consistently.

## Subscript Ordering Convention

**Always**: group (g), state (s), industry (i), time (t), lag (k)

Examples: $\td_{g,t,k}$, $\td_{g,s,t,k}$, $\omega_{gi}^{(t-k)}$

## Core Symbols

| Symbol | LaTeX | Meaning | Defined in |
|--------|-------|---------|------------|
| $L_{i,t}$ | `L_{i,t}` | Labor share of value added, industry i, year t | eq:raw_td |
| $\text{td}_{i,t,k}$ | `\text{td}_{i,t,k}` | Raw task displacement, industry i, year t, lag k | eq:raw_td |
| $\widehat{\text{td}}_{i,t,k}$ | `\widehat{\text{td}}_{i,t,k}` | Adjusted (fitted) task displacement | eq:wls_projection |
| $\text{td}^{\text{GEA}}_{i,t,k}$ | `\text{td}^{\text{GEA}}_{i,t,k}` | GE-adjusted task displacement | eq:gea |
| $\Delta\text{AP}^{a}_{i,t,k}$ | `\Delta\text{AP}^{a}_{i,t,k}` | Rising share in costs, automation proxy a | eq:automation_proxy |
| $K^{a}_{i,t}$ | `K^{a}_{i,t}` | Capital services of type a | eq:automation_proxy |
| $VA_{i,t}$ | `VA_{i,t}` | Value added | eq:automation_proxy |
| $\omega_{gi}^{(t-k)}$ | `\omega_{gi}^{(t-k)}` | Industry exposure weight, group g, industry i, base year t-k | eq:omega |
| $\text{RCA}_{gi}^{(t-k)}$ | `\text{RCA}_{gi}^{(t-k)}` | Routine cognitive adjustment factor | eq:rca |
| $\text{RoutineShare}_{gi}^{(t-k)}$ | `\text{RoutineShare}_{gi}^{(t-k)}` | Fraction of group hours in routine occupations | eq:rca |
| $\text{RTI}$ | `\text{RTI}` | Routine task intensity index | eq:rti |
| $r_{\text{cog}}, r_{\text{man}}$ | `r_{\text{cog}}, r_{\text{man}}` | Routine cognitive/manual task content | eq:rti |
| $nr_{\text{cog,anal}}, nr_{\text{man,phys}}$ | `nr_{\text{cog,anal}}, nr_{\text{man,phys}}` | Non-routine cognitive analytical / manual physical | eq:rti |
| $\td_{g,t,k}$ | `\td_{g,t,k}` | Group-level task displacement (national) | eq:group_td |
| $\td_{g,s,t,k}$ | `\td_{g,s,t,k}` | Group-level task displacement (state) | eq:state_td |

## Regression Symbols

| Symbol | LaTeX | Meaning | Defined in |
|--------|-------|---------|------------|
| $Y_{g,t}$ | `Y_{g,t}` | Political outcome, national | eq:twfe_national |
| $Y_{g,s,t}$ | `Y_{g,s,t}` | Political outcome, state-level | eq:twfe_state |
| $\alpha_g$ | `\alpha_g` | Group fixed effect | eq:twfe_national |
| $\delta_s$ | `\delta_s` | State fixed effect | eq:twfe_state |
| $\gamma_t$ | `\gamma_t` | Year fixed effect | eq:twfe_national |
| $\alpha_{gs}$ | `\alpha_{gs}` | Group-by-state fixed effect | eq:twfe_robust |
| $\beta_k$ | `\beta_k` | Coefficient of interest at lag k | eq:twfe_national |
| $\bar{Y}^{\text{IVW}}_{g,t}$ | `\bar{Y}^{\text{IVW}}_{g,t}` | Inverse-variance-weighted group mean | eq:ivw |
| $\est$ | `\est` | IRT ideology estimate (custom command) | subsec:outcomes |

## Custom LaTeX Commands

| Command | Expansion | Usage |
|---------|-----------|-------|
| `\td` | TD | Task displacement in math mode |
| `\rti` | RTI | Routine task intensity in math mode |
| `\est` | *est* (italic) | IRT estimate variable |
| `\note{...}` | Italicized note | Table notes |

## Parameters

| Symbol | Value | Meaning |
|--------|-------|---------|
| $\pi$ | 0.30 | Share of tasks amenable to automation |
| $\lambda$ | 0.40 | Elasticity of substitution (routine/non-routine) |
| $k$ | {1, 2, 3} | Lag window in years |

## Panel Dimensions

| Panel | Formula | Size |
|-------|---------|------|
| National | 18 groups x 9 years x 3 lags | 486 obs |
| State | 49 states x 18 groups x 9 years x 3 lags | 23,814 obs |
