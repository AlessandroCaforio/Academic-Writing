---
name: generate-maps
description: Regenerate all CZ choropleth maps for the thesis (Frey et al. style)
---

# Generate Maps

Regenerate all commuting-zone choropleth maps used in the thesis. Maps follow
the visual style of Frey, Berger & Chen (2018) "Political Machinery":
borderless CZ polygons, thin state boundary overlays, sequential warm palette
for task displacement, diverging blue-red for ideology.

## Arguments

- (no args) — regenerate all 4 standard figures
- `election-td` — only the 2x2 election-year TD maps
- `election-ideo` — only the 2x2 election-year ideology maps
- `allyears-td` — only the 2x4 all-years TD maps (appendix)
- `allyears-ideo` — only the 2x4 all-years ideology maps (appendix)
- `single --year YYYY --var COLNAME` — one full-size map

## Steps

1. Run the map generation command:

```bash
cd /Users/alessandro/Projects/Tesi && python3 -m src.geo_viz <COMMAND>
```

Where `<COMMAND>` is one of: `all`, `election-td`, `election-ideo`, `allyears-td`, `allyears-ideo`, `single --year YYYY --var td_cz`.

If no arguments are provided, use `all`.

2. Report which figures were saved to `results/figures/`.

3. If the thesis needs recompilation (because figure files changed), run:

```bash
cd /Users/alessandro/Projects/Tesi/thesis && tectonic main.tex 2>&1 | tail -5
```

## Output Files

| Command | Output |
|---------|--------|
| `election-td` | `fig_cz_map_td_elections.pdf/.png` |
| `election-ideo` | `fig_cz_map_ideo_elections.pdf/.png` |
| `allyears-td` | `fig_cz_map_td_all_years.pdf/.png` |
| `allyears-ideo` | `fig_cz_map_ideo_all_years.pdf/.png` |
| `single` | `fig_cz_map_{var}_{year}.pdf/.png` |

## Dependencies

- `src/geo_viz.py` — MapEngine class
- `data/reference/cz_boundaries.geojson` — CZ polygons (722 CZs, Albers projection)
- `data/reference/state_boundaries.geojson` — State outlines (49 states)
- `results/panels/cz_analysis_panel.csv` — CZ-level panel data
