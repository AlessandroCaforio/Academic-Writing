# Project Brain - Thesis: Automation & Political Ideology

> This file is automatically loaded by Claude Code. Add notes here to preserve knowledge across sessions.

## Memory Protocol

**How to save**: Use `brain: [your note]` prefix to add something to this file.
**Proactive mode**: Claude will suggest saving important decisions, data quirks, or findings discovered during work.

**Student**: Alessandro Caforio - Bocconi University, Master in PPA
**Supervisor**: Professor Massimo Anelli
**Research Question**: Do workers in high-automation-exposure occupations shift toward populist/conservative political views?

---

## GitHub Repository

**URL**: https://github.com/AlessandroCaforio/Tesi-Automation-Political-Ideology
**Visibility**: Private (share with professor via collaborator invite)
**MCP**: GitHub MCP configured for this project (scope: project)

---

## Project Structure (Updated 2026-02-16)

```
Tesi/
├── analysis/                              # Core analysis notebooks
│   ├── 01_task_displacement/              # Task displacement measure pipeline
│   │   ├── 01_labor_share.ipynb           # BEA-KLEMS → industry TD (delta)
│   │   ├── 01b_foreign_labor_share.ipynb  # EU KLEMS foreign TD (diagnostic — IV ruled out)
│   │   ├── 02_acs_weights.ipynb           # ACS 2002 + O*NET → ω, RCA weights
│   │   ├── 03_group_aggregation.ipynb     # ω × RCA × td → group-level TD
│   │   └── archive/                       # Old cumulative & exploratory (git-ignored)
│   ├── 02_cces_analysis/                  # CCES political outcomes
│   │   ├── 01_panel_construction.ipynb    # CCES + IRT merge, panel assembly
│   │   ├── 02_twfe_analysis.ipynb         # TWFE regressions
│   │   └── archive/                       # Old versions (git-ignored)
│   └── 03_cz_analysis/                    # Commuting zone analysis
│       └── cz_analysis.ipynb              # CZ-level shift-share + TWFE
│
├── data/
│   ├── raw/                               # Large data (git-ignored)
│   │   └── README.md                      # Download instructions
│   ├── processed/                         # Generated data (git-ignored)
│   │   ├── acs_2002_with_measures.csv     # ACS microdata + task measures
│   │   ├── china_import_penetration.csv   # Shift-share control
│   │   └── markup_by_industry.csv         # Industry markups control
│   └── reference/                         # Reference data (tracked)
│       ├── BEA-BLS-industry-level-production-account-1997-2023.xlsx
│       ├── AR_automation_proxies_1997_2016.csv
│       ├── onet_task_measures_5digit.csv
│       ├── crosswalk_consolidated_bea_naics3.csv
│       ├── crosswalk_bea_nace.csv         # BEA ↔ NACE Rev 2 mapping (diagnostic)
│       ├── industry_controls_panel.csv
│       ├── survey_idealpoints_2022.csv    # IRT ideology estimates (75 MB)
│       ├── bls_excel/                     # BLS automation proxies
│       ├── euklems/                       # EU KLEMS data (diagnostic — IV ruled out)
│       ├── cz_boundaries.geojson          # 722 CZ polygons (Albers, from county dissolve)
│       ├── state_boundaries.geojson       # 49 state outlines (continental US)
│       └── cb_2020_us_county_20m.zip      # Census county shapefile (source)
│
├── results/
│   ├── intermediate/                      # Pipeline artifacts (git-ignored)
│   ├── panels/                            # Analysis-ready panels (git-ignored)
│   ├── figures/                           # Current figures
│   │   └── archive/                       # Old/diagnostic figures (git-ignored)
│   ├── tables/                            # LaTeX tables
│   └── summary_stats/                     # CCES panel outputs
│
├── thesis/                                # LaTeX thesis
│   ├── main.tex
│   ├── chapters/
│   ├── references.bib
│   └── figures/
│
├── docs/
│   └── ARCHIVE/                           # Archived exploratory notebooks
│
├── src/                                   # Python utilities
│   ├── utils.py                           # Data loading, SOC matching, RTI, demo groups
│   └── geo_viz.py                         # CZ choropleth maps (MapEngine class)
│
├── requirements.txt
├── LICENSE (MIT)
├── README.md
└── CLAUDE.md (this file, git-ignored)
```

---

## Data Sources

### ACS Data (Labor Market)
- **Path**: `data/raw/usa_00021.csv`, `usa_00022.csv`, `usa_00023.csv`
- **Source**: IPUMS USA (2002-2022)
- **Purpose**: Calculate omega (industry exposure) and RCA_routine components
- **Key variables**: EDUC/EDUCD, OCC (OCCSOC), IND (NAICS), INCWAGE, PERWT
- **Quirks**:
  - Use 5-digit "broad SOC" for occupation matching (83.4% match rate)
  - IPUMS OCCSOC format: 412010 -> truncate to 41201 for matching

### CCES Data (Political Attitudes)
- **Raw file**: `CCES_python/cumulative_2006-2024.csv` (283 MB)
- **IRT estimates**: `data/reference/survey_idealpoints_2022.csv` (75 MB)
- **Years**: 2006, 2008, 2010, 2012, 2014, 2016, 2018, 2020, 2022
- **Key variables**: IRT ideology estimate (`est`), demographics
- **Note**: IRT estimates from external project, merged via case_id + year

### BEA-BLS KLEMS (Labor Share)
- **Path**: `data/reference/BEA-BLS-industry-level-production-account-1997-2023.xlsx`
- **Purpose**: Calculate change in labor share by industry-year
- **Formula**: `td_YYYY_2002 = ln(labor_share_YYYY) - ln(labor_share_2002)`

### O*NET 30.1 (Task Measures)
- **Path**: `data/reference/onet_task_measures_5digit.csv`
- **Use**: Identify routine occupations (top 1/3 by RTI)
- **RTI formula**: `RTI = (r_cog + r_man) - (nr_cog_anal + nr_man_phys)`

### EU KLEMS INTANProd (Diagnostic — IV Ruled Out)
- **Path**: `data/reference/euklems/national_accounts.csv`
- **Source**: [EU KLEMS INTANProd (LUISS)](https://euklems-intanprod-llee.luiss.it/)
- **Purpose**: Attempted as IV for US `td_raw` — **ruled out** (negative first-stage coefficient)
- **Countries**: DE, FR, UK, JP
- **Why it failed**: Labor share is an equilibrium outcome (technology + institutions + policy). During major shocks (Great Recession, COVID), US labor share falls (flexible markets, layoffs) while European labor share rises (Kurzarbeit, labor hoarding). Same shock, opposite institutional response → instrument captures institutional differences, not shared technology. Contrast with A-R/ADH IVs which instrument technology/supply variables (robot adoption, Chinese exports) that are globally driven.
- **Notebook**: `analysis/01_task_displacement/01b_foreign_labor_share.ipynb` documents full diagnostic

---

## Measure Pipeline

### Step 1: Industry Task Displacement (BEA-KLEMS)
- **Notebook**: `analysis/01_task_displacement/01_labor_share.ipynb`
- **Data**: BEA-BLS KLEMS labor share (1997-2023)
- **Approach**: Delta/rolling-window — `td_{i,t,k} = ln(L_{i,t}) - ln(L_{i,t-k})` for k ∈ {1,2,3}
- **Primary measure**: `td_raw` (raw labor share change, sign-flipped)
- **Controls**: China import penetration + industry markups added as explicit shift-share controls in TWFE
- **Note**: `td_adjusted` (WLS projection onto automation proxies) available but NOT used as primary
- **Output**: `results/intermediate/industry_task_displacement_ddelta.csv`

### Step 2: ACS Weights (O*NET + Industry Mapping)
- **Notebook**: `analysis/01_task_displacement/02_acs_weights.ipynb`
- **Data**: ACS microdata (usa_00023.csv)
- **Sample**: Age 16-65, excl. institutional GQ, excl. AK/HI, wage>0
- **Weights**: Time-varying ω_{gi}^{(t-k)} and RCA_{gi}^{(t-k)} for each base year
- **Output**: `data/processed/acs_2002_with_measures.csv`, `results/intermediate/delta_weights_by_year*.csv`

### Step 3: Group-Level Task Displacement
- **Notebook**: `analysis/01_task_displacement/03_group_aggregation.ipynb`
- **Formula**: `TD_{g,t,k} = Σ_i ω_{gi}^{(t-k)} × RCA_{gi}^{(t-k)} × td_{i,t,k}`
- **Groups**: 18 demographic groups (Edu × Gender × Age)
- **Outputs**:
  - `results/panels/group_task_displacement_panel_delta.csv` (national: 486 obs)
  - `results/panels/state_group_task_displacement_panel_delta.csv` (state: 23,814 obs)

### Step 4: Panel Construction
- **Notebook**: `analysis/02_cces_analysis/01_panel_construction.ipynb`
- **Data**: CCES cumulative + IRT estimates + task displacement panel
- **Output**: `results/panels/panel_state_group.csv`

### Step 5: TWFE Regression Analysis
- **Notebook**: `analysis/02_cces_analysis/02_twfe_analysis.ipynb`
- **Primary outcome**: `est` (IRT ideology — continuous latent spectrum)
- **Secondary outcome**: `pct_republican` (vote share — behavioral robustness check)
- **Models**:
  - National: `Y_{g,t} = α_g + γ_t + β_k × TD_{g,t,k} + ε_{g,t}`
  - State-level: `Y_{g,s,t} = α_g + δ_s + γ_t + β_k × TD_{g,s,t,k} + ε_{g,s,t}`
  - Robustness: `Y_{g,s,t} = α_{gs} + γ_t + β_k × TD_{g,s,t,k} + ε_{g,s,t}` (state×group FE)

### Step 6: Commuting Zone Analysis
- **Notebook**: `analysis/03_cz_analysis/cz_analysis.ipynb`
- **Approach**: Shift-share at CZ level using ACS industry employment shares
- **Output**: `results/panels/cz_*.csv`

---

## Code Decisions

### Automation Measure: A-R-K Task Displacement
- **Why**: Captures actual displacement mechanism, not just RTI correlation
- **Reference**: Acemoglu, Restrepo & Kong (2024) "Tasks At Work" - NBER WP 32872
- **Formula**: `Task_displacement_g = SUM_i(omega_gi * td_i)`

### SOC Matching: 5-digit Broad SOC
- **Problem**: IPUMS OCCSOC and O*NET SOC use different versions
- **Solution**: Truncate to 5-digit broad SOC codes
- **Result**: 83.4% match rate (acceptable)
- **Example**: O*NET 41-2011.00 -> 41201, IPUMS 412010 -> 41201

### 18 Demographic Groups
| Dimension | Levels |
|-----------|--------|
| Education | HS or less, Some college, College+ |
| Gender | Male, Female |
| Age | 18-34, 35-54, 55+ |

### IRT for Ideology
- **Source**: External IRT estimates (`survey_idealpoints_2022.csv`)
- **Variable**: `est` (ideology score, higher = more conservative)
- **Merge**: CCES case_id + year
- **Implementation**: `analysis/04_cces_analysis/CCES_automation_analysis.ipynb`

### State-Level Geographic Variation (implemented 2026-01-16)
- **Approach**: Add state dimension to capture local industry composition
- **Formula**: `TD_gst = Σ_i ω_gsi × RCA_gi × td_it`
- **ω (omega)**: Computed WITHIN each state (state-specific industry exposure)
- **RCA**: Kept at NATIONAL level (avoid sparse cells)
- **Panel**: 18 groups × 49 states × 9 years = 7,938 observations
- **Output**: `results/state_group_task_displacement_panel.csv`

### Task Displacement Sign Convention (updated 2026-01-21)
- **Original construction**: `td = ln(labor_share_t) - ln(labor_share_2002)`
  - Declining labor share (automation) → negative values
  - More negative = more automation exposure
- **SIGN FLIPPED**: We multiply by -1 so that **higher values = more automation exposure**
- **Rationale**:
  - Intuitive interpretation: "higher TD = more exposed to automation"
  - Regression coefficients: β > 0 means "more automation → more conservative"
  - Consistent with other exposure measures (China shock, robot exposure)
- **Implementation**: Sign flip applied in `group_aggregation.ipynb` before saving panels
- **Result**: td_raw now ranges from ~-0.10 to ~0.15 (sign-flipped, positive = more displacement)

### TWFE Specification (updated 2026-02-14)
**Primary measure**: `td_raw` with explicit controls for import competition and markups

**Main specification:**
```
Ideology_gst = α_g + δ_s + γ_t + β × TD_raw_gst + θ₁ × China_gst + θ₂ × Markup_gst + ε_gst
```
- α_g = Group FE (18 dummies) - absorbs education/gender/age effects
- δ_s = State FE (50 dummies) - absorbs time-invariant state characteristics
- γ_t = Year FE (9 dummies) - absorbs common time trends
- China_gst = shift-share China import penetration exposure
- Markup_gst = shift-share industry markup change exposure
- β identified from: within-group, within-state, over-time variation in TD, net of trade/markup confounds

**Robustness specification:**
```
Ideology_gst = α_gs + γ_t + β × TD_raw_gst + θ₁ × China_gst + θ₂ × Markup_gst + ε_gst
```
- α_gs = Group×State FE (900 dummies) - most demanding specification

**Why td_raw + controls instead of td_adjusted:**
- td_adjusted (WLS projection) bakes in researcher degrees of freedom about which proxies capture "automation"
- td_raw + explicit controls is more transparent — the reader sees exactly what is controlled for
- Coefficients now have expected sign: β > 0 (more automation → more conservative)

**Note on pseudo-panel:**
- CCES is repeated cross-section, NOT individual panel
- Cannot do individual FE; use group FE (synthetic cohorts approach)
- Reference: Deaton (1985) "Panel data from time series of cross-sections"

---

## Key Notebooks (6 Total)

### Task Displacement Pipeline (`analysis/01_task_displacement/`)
| Step | Notebook | Output |
|------|----------|--------|
| 1 | `01_labor_share.ipynb` | `results/intermediate/industry_task_displacement_ddelta.csv` |
| 2 | `02_acs_weights.ipynb` | `data/processed/acs_2002_with_measures.csv` |
| 3 | `03_group_aggregation.ipynb` | `results/panels/*_panel_delta.csv` |

### CCES Analysis (`analysis/02_cces_analysis/`)
| Step | Notebook | Purpose |
|------|----------|---------|
| 4 | `01_panel_construction.ipynb` | CCES + IRT merge, panel assembly |
| 5 | `02_twfe_analysis.ipynb` | TWFE regressions |

### CZ Analysis (`analysis/03_cz_analysis/`)
| Step | Notebook | Purpose |
|------|----------|---------|
| 6 | `cz_analysis.ipynb` | CZ-level shift-share + TWFE |

### Archived Notebooks
- `analysis/*/archive/` — superseded versions of pipeline notebooks (git-ignored)
- `docs/ARCHIVE/` — older exploratory notebooks from early iterations


---

## Project Status

### Completed
- [x] O*NET task measures construction
- [x] SOC matching solution (5-digit broad SOC, 83.4%)
- [x] CCES data harmonization (2006-2022)
- [x] ACS data preparation
- [x] BEA-BLS labor share data
- [x] Cumulative task displacement measures (td_2002-YYYY)
- [x] CCES + IRT ideology merge
- [x] TWFE validation checks (sufficient within-group variation)
- [x] GitHub repository setup
- [x] Project structure reorganization
- [x] Notebook pipeline cleanup (6 core notebooks)
- [x] State-level task displacement panel
- [x] Initial TWFE regression results
- [x] CZ-level shift-share analysis
- [x] Project structure reorganization (2026-02-13)
- [x] Measure switch: td_raw + controls (2026-02-14)
- [x] Foreign Labor Share IV: attempted and ruled out (2026-02-16) — negative first-stage coefficient due to institutional divergence (see `01b_foreign_labor_share.ipynb`)

### In Progress
- [ ] Introduction revision (IRT as primary outcome, shift-share framing)
- [ ] Robustness checks and heterogeneous effects

### Pending
- [ ] Abstract
- [ ] Conclusion
- [ ] Final tables and write-up for professor

---

## Literature Support for Research Design

### Shift-Share (Bartik) Identification
- **Status**: Well-established in labor/trade economics
- **Precedents**: Autor, Dorn & Hanson (2013) China Shock; Acemoglu & Restrepo (2020) robot exposure
- **Your approach**: Delta/rolling-window shift-share at both group level and CZ level
- **Key**: Weights are predetermined (from base year t-k), shifts are industry-level shocks

### Cultural Backlash Mechanism (Economic Shock → Cultural Response)

**Core argument**: Economic shocks affect CULTURAL attitudes, not primarily economic policy preferences.

| Paper | Finding | Use in thesis |
|-------|---------|---------------|
| Colantone & Stanig (2021) *JEPS* | Trade shocks → nationalist attitudes | Primary support for mechanism |
| Mutz (2018) *PNAS* | Status threat, not economic hardship, drove 2016 vote | Theoretical foundation |
| Manunta et al. (2025) *PSPB* | Identity threat mediates both economic and cultural paths | Mediation framework |
| Fiorina (2024) Hoover | Cultural values affected by economic distress | Reviews debate |

### Group-Level Treatment Assignment Defense

**Defense strategy**:
1. **Theoretical**: Treatment IS group-level; automation displaces occupational-demographic groups, not random individuals
2. **Precedent**: ADH (2013) uses commuting zones; A-R (2020) uses industry-regions; your 18-group design follows same logic
3. **Methodology**: Gary King's ecological inference bounds can provide robustness
4. **Mechanism**: Status threat operates through group identity ("non-college workers" as social category)

---

## Key Findings

### Matching Statistics
- SOC match rate: 83.4% using 5-digit broad SOC
- Industry match rate: 92.9% using consolidated_bea

### TWFE Validation (from cces_cumulative.ipynb)
- Panel: 144 group-year observations (18 groups x 8 years)
- Balanced: All groups observed in all years
- Min cell size: 501 observations
- Within-group variation: 22.3% of total (sufficient for TWFE)
- Residual variation after FEs: 7.6% (sufficient)

### Main Regression Results (td_raw + controls, updated 2026-02-16)
- **Primary measure**: `td_raw` with china_exposure + markup_exposure as controls
- **Primary outcome**: IRT ideology (`ideology_ivw`)
- **Secondary outcome**: Republican vote share (`pct_republican`) — behavioral robustness check
- **IRT Ideology, Spec 3 (Group + State + Year FE), k=1**: β = 0.468, SE = 0.201, p = 0.020
- **IRT Ideology, Spec 4 (State×Group + Year FE), k=1**: β = 0.455, SE = 0.205, p = 0.026
- **Lag comparison (IRT, Spec 3)**: k=1: β=0.468**, k=2: β=0.290*, k=3: β=0.237†
- **pct_republican, Spec 3, k=1**: β = 0.394, SE = 0.211, p = 0.062 (corroborates IRT)
- **Coefficient sign**: β > 0 (more automation → more conservative), as expected
- **Note**: IRT ideology results are stronger and more statistically significant than Republican vote share

---

## Important Reminders

### DO use:
- `td_raw` as primary task displacement measure (NOT `td_adjusted`)
- China import penetration + industry markups as explicit shift-share controls
- Delta/rolling-window measures (td_{i,t,k} for k ∈ {1,2,3}), not cumulative
- RTI only to identify routine occupations (top 1/3)
- 18 demographic groups (edu x gender x age) as unit of analysis
- 5-digit SOC matching for occupation codes
- IRT `est` variable as **PRIMARY** outcome for ideology
- `pct_republican` as **SECONDARY** outcome (behavioral robustness check only)
- Shift-share (Bartik) framing for identification at both group and CZ levels

### DO NOT suggest:
- `td_adjusted` as primary measure (use `td_raw` + controls instead)
- Foreign labor share IV (ruled out — negative first-stage due to institutional divergence; see `01b_foreign_labor_share.ipynb`)
- Robot exposure shift-share (replaced by task displacement)
- Individual-level treatment (groups are the unit)
- 6-digit SOC matching (5-digit is the solution)
- Republican vote share as the main/headline finding (it is secondary to IRT)
- Self-reported ideology scales (use IRT estimates)

---

## Session Log

| Date | Summary |
|------|---------|
| 2025-01-08 | Created project brain (CLAUDE.md) |
| 2025-01-09 | Added literature support for cumulative exposure, cultural backlash mechanism, and group-level treatment defense |
| 2025-01-10 | Created GitHub repo, reorganized project structure, updated notebook paths, added IRT ideology data |
| 2025-01-11 | Documented full measure pipeline (5 steps: BEA-KLEMS → ACS omega → Group TD → CCES panel → TWFE) |
| 2026-01-21 | Notebook cleanup: reorganized to 4 core notebooks, archived 12 exploratory notebooks to docs/ARCHIVE/ |
| 2026-01-21 | **Sign convention change**: Flipped TD sign so higher values = more automation exposure. β now positive (automation → conservative). |
| 2026-02-13 | **Project reorganization**: Renumbered analysis folders (01/02/03), step-prefixed core notebooks, created results/intermediate + results/panels, archived old figures, cleaned root junk. |
| 2026-02-14 | **Measure switch**: Changed primary TD measure from `td_adjusted` to `td_raw` + explicit controls (china_exposure, markup_exposure). Re-ran all specs. Coefficients now positive (β=0.291 for spec 3). Updated TWFE explainer, results CSV, empirical_data.json, and CLAUDE.md. |
| 2026-02-16 | **Documentation cleanup**: Clarified IRT as primary outcome (not Republican vote share). Updated README.md (delta methodology, correct folder structure). Deleted temp workspace dirs. Updated thesis/CLAUDE.md chapter status. Fixed outcome hierarchy throughout CLAUDE.md files. |
| 2026-02-16 | **Foreign Labor Share IV — attempted and ruled out**: Downloaded EU KLEMS data, built BEA↔NACE crosswalk, computed td_foreign. Industry-level first stage weak (F<10); group-level first stage has strong F (22-76) but **negative** coefficient. Root cause: labor share is an equilibrium outcome — during shocks (Great Recession, COVID), US labor share falls (flexible markets) while European rises (Kurzarbeit). Contrast with A-R/ADH IVs which instrument technology variables (globally driven). Diagnostic preserved in `01b_foreign_labor_share.ipynb`; all downstream IV code reverted from pipeline notebooks. |
| 2026-02-17 | **CZ maps + geo_viz module**: Created `src/geo_viz.py` (MapEngine class) for Frey-style CZ choropleths. Built CZ/state boundaries from Census county shapefile. Added `/generate-maps` skill and `map-generator` agent. Maps in Ch.3 (election years) and Appendix B (all years). |
