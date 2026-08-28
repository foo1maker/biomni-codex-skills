# Source workflows for chromatin-and-epigenomic-analysis

Select the narrowest applicable workflow. Each deployment copy is self-contained and mirrors the canonical package under `04_claude_science_skills/_source_workflows/`.

- [`chip-atlas-diff-analysis`](chip-atlas-diff-analysis/WORKFLOW.md) — Compare two groups of public experiments through ChIP-Atlas to identify differential peak regions or differentially methylated regions with effect sizes, significance statistics, QC warnings, plots, and tabular results.
- [`chip-atlas-peak-enrichment`](chip-atlas-peak-enrichment/WORKFLOW.md) — Identify public ChIP-seq experiments and aggregated factors whose peaks are enriched near an input gene set using the official ChIP-Atlas enrichment analysis, with significance, fold enrichment, overlap metrics, plots, and interpretation safeguards.
- [`scatac-multiome-analysis`](scatac-multiome-analysis/WORKFLOW.md) — Analyze fragments-first single-cell chromatin-accessibility data from QC through LSI, clustering, per-cluster peak recall, gene-activity annotation, and differential accessibility.
