---
name: compile-thesis
description: Compile the thesis with tectonic and report results
disable-model-invocation: true
---

# Compile Thesis

Run the following command and report its output:

```bash
cd /Users/alessandro/Projects/Tesi/thesis && tectonic main.tex 2>&1
```

After running:

1. If compilation **succeeds**: Report "Compilation successful" and list any warnings (undefined references, overfull hboxes, etc.)
2. If compilation **fails**: Report the error with the relevant line number and file, then suggest a fix
3. Count pages if possible: `pdfinfo main.pdf 2>/dev/null | grep Pages`
