# Scrnaseq Seurat Core Analysis

Source workflow: `scrnaseq-seurat-core-analysis`  
Parent Claude Science skill: `single-cell-rna-seq-analysis`

## Purpose

Process single-cell RNA-seq data through quality control, normalization, clustering, cell-type annotation, and visualization using Seurat v5.

## When to use

- Analyze 10X Chromium, Drop-seq, Smart-seq2, or inDrop single-cell RNA-seq data with adaptive QC and doublet detection.
- Integrate multiple batches, identify cell populations, annotate cell types, and compare conditions with replicated pseudobulk analysis.

## Inputs

- Raw or filtered count matrix in CellRanger directory, H5, Seurat RDS, CSV, or TSV format. (required)
- Sample metadata containing sample identifiers, conditions, batches, or donor identifiers. (optional)

## Outputs

- Annotated Seurat object with normalized data, QC metrics, clusters, annotations, and dimensional reductions.
- Normalized matrices, cell metadata, PCA and UMAP coordinates, marker tables, pseudobulk differential-expression tables, integration diagnostics, and visualizations.

## Workflow

1. Optionally correct ambient RNA for raw or high-soup data, then load counts, calculate QC metrics, detect doublets, and filter cells.
2. Normalize with SCTransform or LogNormalize, run PCA, and select an evidence-based dimensionality.
3. Integrate multiple batches when present and evaluate mixing while preserving cell-type structure.
4. Cluster at multiple resolutions, compute UMAP, identify markers, and visualize cell populations.
5. Annotate cell types and, for replicated multi-sample data, aggregate counts and run pseudobulk differential expression.
6. Export the processed object, normalized counts, metadata, coordinates, markers, and summary statistics.

## Decision rules

- Use SoupX only for raw matrices or high-soup tissues; skip it for filtered PBMC data.
- Prefer SCTransform for UMI data and batch effects; use LogNormalize for speed or non-UMI data.
- Use Harmony for fast simple integration, CCA for complex batches, and RPCA for datasets larger than 100,000 cells.
- Use at least 15 principal components; test resolutions 0.4, 0.6, 0.8, and 1.0 and select by biology and stability.
- Use pseudobulk DESeq2 for inferential condition comparisons; marker-level cell tests are exploratory.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_699e7305b308861b` — Seurat v5: Use for the R-based core single-cell workflow.

### Secondary resources

- `rr_8ee969106d66a56a` — Harmony, Seurat CCA or RPCA, DoubletFinder, SoupX, SingleR, and DESeq2: Use as needed for integration, doublet and ambient-RNA handling, annotation, and pseudobulk inference.

### Fallback resources

- `rr_7e53b8edb3f2125b` — LogNormalize: Use when SCTransform exhausts memory on a large dataset or when a faster classic workflow is preferred.

### Optional resources

- `rr_7dadc66200d982df` — Seurat: Core single-cell analysis framework.
- `rr_57aa6a24693fe13a` — DoubletFinder: Batch-aware doublet detection.
- `rr_ae8537b323692784` — harmony: Fast batch integration.
- `rr_174399f6317a39a7` — SoupX: Ambient RNA correction for raw or high-soup data.
- `rr_8a7a79093107ed07` — DESeq2: Pseudobulk differential-expression inference.
- `rr_3a5322f8dd5b3ed0` — SingleR: Automated reference-based cell-type annotation.

## Validation / QC

- Aim for more than 70 percent cell retention after doublet detection and filtering.
- Verify PCA feature usage and cumulative variance, then use the elbow and suggested principal-component count.
- Assess integration with batch LISI while preserving cell-type structure.
- Inspect marker results before saving and distinguish exploratory marker tests from inferential pseudobulk comparisons.
- Evidence requirement: Choose clustering resolution by biology and stability rather than by a single visual separation criterion.
- Evidence requirement: Condition-level claims require sample-level pseudobulk aggregation and an appropriate design formula.

## Failure handling

- Overly strict QC thresholds produce low cell retention.
- Using the wrong reduction or too few principal components produces poor separation or batch-driven clusters.
- SCTransform can exhaust memory for datasets larger than about 50,000 cells.
- Fallback rule: Use MAD-based filtering instead of fixed thresholds when cell retention is too low.
- Fallback rule: Use LogNormalize or process in batches when SCTransform exceeds available memory.
- Fallback rule: Skip integration for a single batch; otherwise run integration before clustering on an integrated reduction.

## Limitations

- The workflow is not intended for bulk RNA-seq, Python-based single-cell analysis, or spatial transcriptomics.
- Inferential pseudobulk analysis is limited to multi-sample designs with suitable replication and metadata.

## Important domain-specific rules

- Ambient-RNA decision logic, batch-aware QC, doublet detection, and retention checkpoints.
- Normalization and integration selection, dimensionality checks, multi-resolution clustering, annotation, and pseudobulk distinction.

## Portability boundary

- Mandatory internal script invocation, verification-message enforcement, internal path rules, and packaged export orchestration. — migration action: `exclude_or_capability_map`
- Fixed platform report-layout directives and generated analysis-report packaging. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
