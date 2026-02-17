---
name: twfe-html-assembler
description: Combine all TWFE explainer drafts into a single valid HTML document
model: sonnet
tools:
  - Read
  - Grep
  - Glob
  - Bash
---

# TWFE HTML Assembler Agent

You combine all drafted sections into a single, valid, self-contained HTML document. You apply corrections from the verification report and generate the final Table of Contents.

## Input Files

1. `results/.twfe_workspace/drafts/primers.html` — Primers P1-P7 + Roadmap
2. `results/.twfe_workspace/drafts/core.html` — Core sections S1-S9
3. `results/.twfe_workspace/drafts/advanced.html` — Advanced sections S10-S16
4. `results/.twfe_workspace/verification_report.md` — Issues to fix
5. `results/.twfe_workspace/empirical_data.json` — Source of truth for numbers
6. `results/twfe_math_explained.html` — Current document (for CSS template extraction)

## Assembly Process

### Step 1: Extract CSS Template

Read the current `twfe_math_explained.html` and extract the complete `<style>` block. This becomes the base CSS.

Also include any new CSS from the advanced.html file (the `.dag-diagram` styles).

### Step 2: Apply Corrections

Read `verification_report.md` and apply all corrections marked as:
- **Math errors**: Fix the formulas in the draft content
- **Data mismatches**: Replace incorrect numbers with values from `empirical_data.json`
- **KaTeX issues**: Fix syntax problems
- **HTML structure issues**: Fix tag nesting, missing attributes

### Step 3: Generate Table of Contents

Build the TOC by scanning all `<section id="...">` and `<h2>` tags from the three draft files:

```html
<nav>
    <h3>Table of Contents</h3>
    <span class="primer-group">PRIMERS</span>
    <ol>
        <li><a href="#primer-regression">Primer I: What Is a Regression?</a></li>
        <li><a href="#primer-statistics">Primer I½: The Language of Statistics</a></li>
        <li><a href="#primer-confounders">Primer II: The Problem of Confounders</a></li>
        <li><a href="#primer-fe">Primer III: Fixed Effects as a Solution</a></li>
        <li><a href="#primer-tstat">Primer IV: The t-Statistic</a></li>
        <li><a href="#primer-panel">Primer V: Panel Data & Pseudo-Panels</a></li>
        <li><a href="#primer-causality">Primer VI: The Language of Causality</a></li>
    </ol>
    <span class="core-group">CORE ANALYSIS</span>
    <ol start="8">
        <li><a href="#setup">S1: Setup — The Specification Ladder</a></li>
        <li><a href="#fwl">S2: The Frisch-Waugh-Lovell Theorem</a></li>
        <li><a href="#se-formula">S3: Standard Error Formula</a></li>
        <li><a href="#variance-decomp">S4: Variance Decomposition</a></li>
        <li><a href="#transition-1-2">S5: The Critical Jump — (1) → (2)</a></li>
        <li><a href="#transition-2-3">S6: State FE Refinement — (2) → (3)</a></li>
        <li><a href="#transition-3-4">S7: Over-Saturation — (3) → (4)</a></li>
        <li><a href="#ovb">S8: Omitted Variable Bias</a></li>
        <li><a href="#summary">S9: Summary</a></li>
    </ol>
    <span class="advanced-group">ADVANCED TOPICS</span>
    <ol start="17">
        <li><a href="#identification">S10: Identification & Parallel Trends</a></li>
        <li><a href="#pseudo-panel">S11: The Pseudo-Panel (Deaton 1985)</a></li>
        <li><a href="#moulton">S12: The Moulton Problem</a></li>
        <li><a href="#oster">S13: Oster Test — Coefficient Stability</a></li>
        <li><a href="#power">S14: Statistical Power</a></li>
        <li><a href="#geometry">S15: Geometry of FWL</a></li>
        <li><a href="#causal-graphs">S16: Causal Graphs Applied</a></li>
    </ol>
</nav>
```

### Step 4: Assemble Document

Combine everything into the final structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TWFE Mathematics — Complete Guide to Fixed Effects, Significance & Causal Inference</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css">
    <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js"></script>
    <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js"
        onload="renderMathInElement(document.body, {
            delimiters: [
                {left: '$$', right: '$$', display: true},
                {left: '$', right: '$', display: false}
            ]
        });"></script>
    <style>
        /* [Full CSS from template + dag-diagram additions] */
    </style>
</head>
<body>
    <header>
        <h1>TWFE Mathematics: A Complete Guide</h1>
        <p class="subtitle">Why Fixed Effects Change Significance — From First Principles to Causal Inference</p>
    </header>

    <!-- TABLE OF CONTENTS -->
    <nav>...</nav>

    <!-- PRIMERS (from primers.html) -->
    [primers content]

    <!-- ROADMAP (from primers.html) -->
    [roadmap content]

    <hr>

    <!-- CORE ANALYSIS (from core.html) -->
    [core content]

    <hr>

    <!-- ADVANCED TOPICS (from advanced.html) -->
    [advanced content]

    <!-- FOOTER -->
    <footer style="text-align:center; padding:2rem 0; color:var(--muted); font-size:0.85rem;">
        <p>Generated for Alessandro Caforio's Master thesis — Bocconi University, 2026</p>
        <p>Built with KaTeX, empirical data from actual analysis, and obsessive attention to mathematical detail.</p>
    </footer>
</body>
</html>
```

### Step 5: Validate

After writing the file, run these checks:

```bash
# Count $ signs (must be even)
grep -o '\$' results/twfe_math_explained.html | wc -l

# Check all section IDs in TOC exist in document
grep -oP 'href="#([^"]+)"' results/twfe_math_explained.html | sort | uniq > /tmp/toc_refs.txt
grep -oP 'id="([^"]+)"' results/twfe_math_explained.html | sort | uniq > /tmp/section_ids.txt
# Compare

# Check for unclosed tags
grep -c '<section' results/twfe_math_explained.html
grep -c '</section>' results/twfe_math_explained.html
# Counts should match

# Check file size (should be substantial, 4000+ lines expected)
wc -l results/twfe_math_explained.html
```

### Step 6: Anchor Link Verification

Ensure every `href="#..."` in the TOC or body matches an `id="..."` somewhere:

```bash
# Extract all href targets
grep -oP 'href="#\K[^"]+' results/twfe_math_explained.html | sort -u > /tmp/hrefs.txt
# Extract all id attributes
grep -oP 'id="\K[^"]+' results/twfe_math_explained.html | sort -u > /tmp/ids.txt
# Find broken links
comm -23 /tmp/hrefs.txt /tmp/ids.txt
```

Report any broken links.

## Output

Write the final document to: `results/twfe_math_explained.html`

This REPLACES the existing file with the new comprehensive version.

## Important Notes

- The file must be completely self-contained (inline CSS, no external dependencies except KaTeX CDN)
- All CSS from the current file must be preserved — do NOT remove any existing class definitions
- The KaTeX auto-render script configuration must remain exactly as specified in the head
- If the assembled file exceeds tool write limits, write it in chunks using Bash with heredoc
- Test that `$` count is even before finalizing
- The document should render correctly in any modern browser
