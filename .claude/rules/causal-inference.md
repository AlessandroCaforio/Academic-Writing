---
globs: thesis/chapters/04_*.tex, thesis/chapters/05_*.tex
---

# Causal Inference Standards

Standards for econometric methodology in the methodology and results chapters.

## TWFE Requirements

When writing or reviewing TWFE specifications:

1. **Clearly state what variation identifies β**: Within-group, over-time variation in task displacement (national); within-state-group, over-time (state-level)
2. **Parallel trends**: Must be discussed. Pre-trend tests should be mentioned when available
3. **No feedback assumption**: Current ideology does not affect future task displacement. Defense: TD is constructed from BEA production accounts and base-year ACS weights
4. **Fixed effects absorption**: Explicitly state what each FE absorbs
   - α_g: time-invariant group characteristics (education level, gender, age profile)
   - δ_s: time-invariant state characteristics (political culture, urbanization)
   - γ_t: common time trends (national political swings)
   - α_gs: all time-invariant state-group heterogeneity

## Shift-Share (Bartik) Design

The group-level TD is a shift-share instrument:
- **Shares**: ω_{gi}^{(t-k)} — base-year industry employment composition
- **Shifts**: td_{i,t,k} — industry-level automation shocks
- **Pre-determination**: Shares from year t-k cannot be affected by shocks during [t-k, t]
- **Cite**: Borusyak, Hull & Jaravel (2022) for shift-share inference conditions; Goldsmith-Pinkham, Sorkin & Swift (2020) for the shares-based interpretation

## Inference

- National: 18 clusters (group-level). Acknowledge limited clusters — interpret with caution
- State: ~882 clusters (state x group). More reliable inference
- Clustering rationale: serial correlation within groups over time (Deaton 1985 pseudo-panel)

## Pseudo-Panel

- CCES is repeated cross-section, NOT individual panel
- Cannot do individual FE → use group FE (synthetic cohorts)
- Reference: Deaton (1985) "Panel data from time series of cross-sections"
- Cell sizes matter: minimum 501 observations per group-year ensures stable group means

## Common Pitfalls to Flag

- Claiming individual-level causal effects from group-level variation (ecological inference)
- Omitting discussion of parallel trends assumption
- Using "causal" language without proper qualification
- Ignoring the small-cluster problem in national specifications
- Not discussing potential confounders absorbed vs. not absorbed by FEs
