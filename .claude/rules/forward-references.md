# Forward Reference Requirements

Applies when editing `thesis/chapters/02_literature.tex`.

## Purpose

Chapter 2 (Literature Review) must set up every concept that Chapters 3-5 reference back to. Forward references using `\Cref` create a navigable web connecting the review to the technical chapters.

## Required Forward References

The following concepts MUST be introduced in Ch.2 with a forward `\Cref` to where they are developed:

| Concept | Introduce in | Forward ref to | Label |
|---------|-------------|----------------|-------|
| Task-based framework (ALM) | sec 2.1.1 | Ch.3 (O*NET) | `\Cref{ch:data}` |
| Shift-share / Bartik design | sec 2.1.2 | Ch.4 (identification) | `\Cref{ch:methodology}` |
| Task displacement measure | sec 2.1.3 | Ch.4 (construction) | `\Cref{sec:td_measure}` |
| Labor share methodology | sec 2.1.3 | Ch.4 (eq:raw_td) | `\Cref{subsec:industry_td}` |
| Rent dissipation concept | sec 2.1.3 | Ch.5 (heterogeneous) | `\Cref{sec:heterogeneous}` |
| Pseudo-panel approach | sec 2.2.3 | Ch.4 (TWFE) | `\Cref{sec:empirical}` |
| IRT ideology measurement | sec 2.4 | Ch.3 (IRT data) | `\Cref{sec:data_sources}` |
| Demographic group definition | sec 2.4 | Ch.3 (groups) | `\Cref{sec:groups}` |
| Heterogeneous effects | sec 2.3.5 | Ch.5 (subgroups) | `\Cref{sec:heterogeneous}` |

## Syntax

- Start of sentence: `As I detail in \Cref{ch:methodology}, ...`
- Mid-sentence: `...which I formalize in \cref{sec:td_measure}`
- Parenthetical: `(see \Cref{ch:data})`

## Validation

After writing Ch.2, ALL `\Cref` / `\cref` targets must exist in the compiled document. Compilation with `tectonic` will show `Undefined reference` warnings for broken forward refs.
