# Scrnaseq Scanpy Core Analysis

Source workflow: `scrnaseq-scanpy-core-analysis`  
Parent Claude Science skill: `single-cell-rna-seq-analysis`

## Purpose

Process single-cell RNA-seq data through quality control, normalization, clustering, cell-type annotation, and analysis-ready visualization using Scanpy and the scverse ecosystem.

## When to use

- Analyze 10X Chromium, Drop-seq, Smart-seq2, or inDrop single-cell RNA-seq data.
- Integrate multiple batches, annotate cell types, and compare conditions with pseudobulk differential expression when replicate requirements are met.

## Inputs

- Raw or filtered count matrix in CellRanger directory, H5, H5AD, CSV, or TSV format. (required)
- Sample metadata containing sample identifiers, conditions, batches, or donor identifiers. (optional)

## Outputs

- Annotated AnnData object containing raw counts, normalized data, QC metrics, clusters, cell types, PCA, and UMAP coordinates.
- Cell metadata, raw and normalized expression matrices, PCA and UMAP coordinates, marker tables, differential-expression tables, summary text, and visualizations.

## Workflow

1. Load the count data, calculate species-aware QC metrics, detect batch-wise outliers and doublets, and filter cells.
2. Normalize counts, select highly variable genes, scale, run PCA, and integrate multiple batches when needed.
3. Build the neighbor graph, test Leiden resolutions, compute UMAP, identify markers, and annotate cell types.
4. Export the processed object, matrices, metadata, coordinates, markers, and analysis summary.

## Decision rules

- Use MAD-based QC for multi-batch data and fixed tissue-specific thresholds for a single batch.
- Use scVI for complex batch structure and Harmony when speed and simpler integration are priorities.
- Use at least 15 principal components for the neighbor graph; the stated standard default is 30.
- Run pseudobulk differential expression only with at least two samples per condition; treat cell-level Wilcoxon results as exploratory when replication is insufficient.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_540ffcc60b62d741` — Scanpy and AnnData: Use for the Python-based core single-cell workflow.

### Secondary resources

- `rr_0060dc39ee07ca44` — scVI, scANVI, Harmony, CellTypist, Scrublet, and PyDESeq2: Use as needed for integration, annotation, doublet detection, and replicated pseudobulk comparison.

### Fallback resources

- `rr_7e33af66522bc500` — Backed mode or subsampling: Use when a large dataset causes an out-of-memory failure.

### Optional resources

- `rr_063ec900fce6482b` — scanpy: Core single-cell analysis framework.
- `rr_9eb689cc6f292946` — scvi-tools: Complex multi-batch integration.
- `rr_491af9f1229b9aa7` — harmonypy: Fast batch integration.
- `rr_379ab3b524dc4975` — scrublet: Doublet detection.
- `rr_d39a138943e83e9a` — celltypist: Automated reference-based annotation.

## Validation / QC

- Aim for more than 70 percent cell retention after adaptive filtering and doublet removal.
- Inspect PCA loading verification, recommended principal-component count, and LISI diagnostics before downstream clustering.
- Verify marker-table columns and inspect leading marker rows before saving results.
- Evidence requirement: Cross-check automated cell-type labels against marker expression and flag suspect or contaminated labels.
- Evidence requirement: State batch-condition confounding and replication limits when composition or condition comparisons are interpreted.

## Failure handling

- Strict thresholds cause low cell retention.
- Insufficient integration leaves clusters driven by batch.
- Large datasets can exhaust memory or make compressed H5AD export appear stalled.
- Fallback rule: Relax MAD thresholds or use tissue-specific thresholds when retention is too low.
- Fallback rule: Use backed mode or subsampling when memory is insufficient.
- Fallback rule: Retry H5AD saving without compression when export remains slow for more than two minutes.

## Limitations

- The workflow is not intended for bulk RNA-seq, R-based single-cell analysis, or spatial transcriptomics.
- Pseudobulk inference requires replicated samples; cell-level Wilcoxon results are exploratory when only one sample is available.

## Important domain-specific rules

- Batch-aware adaptive QC, doublet detection, and explicit retention checks.
- Normalization, highly-variable-gene selection, PCA, integration diagnostics, multi-resolution clustering, annotation, and pseudobulk decision logic.

## Portability boundary

- Mandatory invocation of packaged internal scripts, verification-message orchestration, and internal result-path conventions. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation handoff for the final report. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
