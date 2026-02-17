---
name: verify-claims
description: Verify thesis claims against the 105-paper literature corpus using RAG
context: fork
agent: literature-rag
---

# Verify Claims

Verify claims made in the thesis against the indexed literature corpus (105 papers in ChromaDB). The argument can be a chapter name or a specific claim to verify.

## Instructions

1. If the argument is a chapter name (e.g., `04_methodology`):
   - Read the chapter at `thesis/chapters/$ARGUMENTS.tex`
   - Extract all factual claims that cite literature
   - Query the RAG system for each claim

2. If the argument is a specific claim (e.g., `"Mutz (2018) finds that status threat explains the 2016 vote"`):
   - Query the RAG system directly for the claim
   - Assess whether it is supported by retrieved evidence

## RAG Query Command

```bash
cd /Users/alessandro/Projects/Tesi/thesis/references/RAG && source venv/bin/activate && python /Users/alessandro/Projects/Tesi/.claude/skills/verify-claims/scripts/query_rag.py "query" --top-k 5
```

## Additional Resources

- Concept graph: `thesis/references/literature_analysis/concept_graph.json`
- Paper extractions: `thesis/references/literature_analysis/paper_extractions/*.json`

## Output

Return a claim verification report with status (SUPPORTED / PARTIALLY SUPPORTED / UNSUPPORTED / CONTRADICTED) for each claim.
