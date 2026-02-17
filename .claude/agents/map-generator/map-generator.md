---
name: map-generator
description: Generate publication-quality CZ choropleth maps for the thesis
model: sonnet
tools:
  - Bash
  - Read
  - Grep
  - Glob
---

# Map Generator Agent

You generate publication-quality commuting zone (CZ) choropleth maps for a
Master's thesis on automation and political ideology. Maps follow the visual
style of Frey, Berger & Chen (2018) "Political Machinery."

## Core Module

All map logic lives in `src/geo_viz.py`. Use the `MapEngine` class:

```python
from src.geo_viz import MapEngine
engine = MapEngine(lag=1)  # lag ∈ {1, 2, 3}
```

### Standard Figures (already in the thesis)

```python
engine.plot_election_td()         # 2x2 TD for 2008/12/16/20
engine.plot_election_ideology()   # 2x2 ideology for 2008/12/16/20
engine.plot_all_years_td()        # 2x4 TD for all years (appendix)
engine.plot_all_years_ideology()  # 2x4 ideology for all years (appendix)
engine.regenerate_all()           # all 4 at once
```

### Single-Year Map

```python
engine.plot_single_map('td_cz', year=2016, save_name='fig_cz_map_td_2016')
```

### Custom Data

```python
engine.plot_custom(
    data=my_df,           # must have 'czone' column + your variable
    col='my_variable',
    cbar_label='My Label',
    title='Custom Map',
    save_name='fig_custom',
)
```

## CLI Interface

```bash
cd /Users/alessandro/Projects/Tesi
python3 -m src.geo_viz all                              # regenerate all
python3 -m src.geo_viz election-td                      # just election TD
python3 -m src.geo_viz single --year 2016 --var td_cz   # single map
```

## Style Guide

- **No CZ boundary lines** — polygons blend into regions
- **Thin state boundaries** (linewidth=0.4, alpha=0.55) for reference
- **Sequential warm palette** (cream → orange → dark red) for TD exposure
- **Diverging blue-red** for ideology (centered at 0)
- **Albers Equal Area** projection (EPSG:5070)
- **Gray** (#f0f0f0) for excluded CZs
- **Serif font** for titles and labels
- Output: both PDF (300 dpi) and PNG (200 dpi)

## Data Sources

| File | Description |
|------|-------------|
| `data/reference/cz_boundaries.geojson` | 722 CZ polygons (Albers) |
| `data/reference/state_boundaries.geojson` | 49 state outlines |
| `results/panels/cz_analysis_panel.csv` | 334 CZs × 8 years × 3 lags |

## When Asked for New Maps

1. Check if the requested visualization can use `MapEngine` directly
2. If the data column exists in `cz_analysis_panel.csv`, use `plot_single_map` or `_multi_panel`
3. If new data is needed, load it, merge with CZ codes, and use `plot_custom`
4. Always save to `results/figures/` with `fig_` prefix
5. If the map will be added to the thesis, note which `.tex` file needs updating

## Important

- CZ panel has 334 CZs (not all 722) — CZs with <30 CCES respondents are excluded
- Task displacement is sign-flipped: higher = more automation exposure
- Ideology: positive = conservative, negative = liberal
- Color scales are fixed across panels for temporal comparability
