# Source workflows for bulk-rna-and-omics-analysis

Select the narrowest applicable workflow. Each deployment copy is self-contained and mirrors the canonical package under `04_claude_science_skills/_source_workflows/`.

- [`bulk-omics-clustering`](bulk-omics-clustering/WORKFLOW.md) — Cluster biological samples or features in normalized quantitative matrices, compare method assumptions, and validate cluster separation and stability.
- [`bulk-rnaseq-counts-to-de-deseq2`](bulk-rnaseq-counts-to-de-deseq2/WORKFLOW.md) — Perform DESeq2 differential-expression analysis on raw RNA-seq integer counts with explicit design, fold-change shrinkage, transformations, and quality control.
- [`deconvolution-bulk-rnaseq`](deconvolution-bulk-rnaseq/WORKFLOW.md) — Estimate cell-type proportions in bulk RNA-seq samples from an annotated single-cell reference and compare composition across groups or timepoints.
- [`rnaseq-fastq-to-counts`](rnaseq-fastq-to-counts/WORKFLOW.md) — Convert raw bulk RNA-seq reads into a differential-expression-ready integer gene-by-sample count matrix with QC, strandedness inference, gene metadata, and a load check, without running the differential-expression contrast.
