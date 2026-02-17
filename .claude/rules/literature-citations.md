# Literature Citation Standards

Applies when editing `thesis/**/*.tex` files, especially `02_literature.tex`.

## Citation Commands

| Context | Command | Example |
|---------|---------|---------|
| Author as subject | `\citet{key}` | `\citet{mutz2018status} finds that...` |
| Parenthetical | `\citep{key}` | `...status threat matters \citep{mutz2018status}` |
| Author possessive | `\citeauthor{key}'s` | `\citeauthor{mutz2018status}'s panel design...` |
| Multiple works | `\citep{k1, k2}` | `\citep{frey2018robot, anelli2019robots}` |
| With page | `\citep[p.~N]{key}` | `\citep[p.~1283]{autor2003skill}` |
| Quote attribution | `\citep[p.~N]{key}` | `...``routine task'' \citep[p.~1283]{autor2003skill}` |

## Verbatim Quote Rules

1. **Every direct quote** (text in `` ``...'' ``) MUST include a page citation: `\citep[p.~N]{key}`
2. Quotes longer than ~15 words should use `\begin{quote}...\end{quote}` with attribution below
3. Use en-dash for page ranges: `pp.~4330--4339`
4. If page number is unknown, use `\citep{key}` without page — but flag for later verification

## BibTeX Key Reference

| Short name | BibTeX key |
|------------|-----------|
| ALM 2003 | `autor2003skill` |
| Autor & Dorn 2013 | `autor2013polarization` |
| A-R 2025 | `acemoglu2025automation` |
| A-R-K 2024 | `acemoglu2024tasks` |
| ADH 2013 | `autor2013china` |
| ADHM 2020 | `autor2020importing` |
| Colantone & Stanig 2018 | `colantone2018trade` |
| Colantone & Stanig 2019 | `colantone2019surge` |
| Colantone & Stanig 2021 | `colantone2021backlash` |
| Frey et al. 2018 | `frey2018robot` |
| Mutz 2018 | `mutz2018status` |
| Morgan 2018 | `morgan2018status` |
| Inglehart & Norris 2016 | `inglehart2016trump` |
| Norris & Inglehart 2019 | `norris2019cultural` |
| Anelli et al. 2019 | `anelli2019robots` |
| Osborne et al. 2023 | `osborne2023psychological` |
| Stenner 2005 | `stenner2005authoritarian` |
| Duckitt & Sibley 2009 | `duckitt2009dual` |
| Manunta et al. 2025 | `manunta2025identity` |
| Gidron & Hall 2017 | `gidron2017politics` |
| Gallego & Kurer 2022 | `gallego2022automation` |
| Kurer 2020 | `kurer2020declining` |
| Im et al. 2019 | `im2019losers` |
| Goos et al. 2014 | `goos2014explaining` |
| Wu 2022 | `wu2022misattributed` |
| Graetz & Michaels 2018 | `graetz2018robots` |
| Dauth et al. 2021 | `dauth2021adjusting` |
| Borusyak et al. 2022 | `borusyak2022quasi` |
| Deaton 1985 | `deaton1985panel` |

## Anti-Patterns

- NEVER write "Author (Year)" manually — always use `\citet{key}`
- NEVER cite a paper not in `references.bib` without flagging it
- NEVER use ibid., op. cit., or similar abbreviations
- NEVER omit page numbers for direct quotes
