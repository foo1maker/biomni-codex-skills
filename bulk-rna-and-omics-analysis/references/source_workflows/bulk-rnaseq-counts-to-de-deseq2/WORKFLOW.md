# Bulk Rnaseq Counts To De Deseq2

Source workflow: `bulk-rnaseq-counts-to-de-deseq2`  
Parent Claude Science skill: `bulk-rna-and-omics-analysis`

## Purpose

Perform DESeq2 differential-expression analysis on raw RNA-seq integer counts with explicit design, fold-change shrinkage, transformations, and quality control.

## When to use

- Estimate differential expression from replicated raw RNA-seq counts.
- Produce shrunken log2 fold changes for ranking and visualization.
- Assess dispersion, sample structure, and effect patterns with standard QC views.

## Inputs

- A genes-by-samples matrix of non-negative raw integer counts. (required)
- Sample metadata with row names matching count-matrix columns and a condition factor with at least two levels. (required)
- Batch, individual, genotype, treatment, or other design covariates. (optional)
- Salmon or Kallisto output through tximport, a SummarizedExperiment, featureCounts or HTSeq output, or a Bioconductor example object. (optional)

## Outputs

- Full and shrunken differential-expression result tables.
- Serialized DESeqDataSet object.
- Size-factor normalized counts and VST or rlog transformed values.
- Dispersion, PCA, MA, and volcano plots.

## Workflow

1. Load raw counts and metadata, validate sample identifiers and integer-count assumptions, pre-filter low-count genes, and set the reference level.
2. Choose a non-confounded design formula for simple, batch-adjusted, paired, or interaction analysis.
3. Fit the DESeq2 model and extract the requested coefficient or contrast.
4. Apply log-fold-change shrinkage for ranking and visualization while retaining unshrunk estimates for hypothesis testing.
5. Generate VST or rlog transformed counts for quality-control and exploratory plots.
6. Review dispersion, PCA, MA, and volcano plots before interpreting differential-expression results.
7. Export result tables, transformed data, significant genes, and the fitted analysis object.

## Decision rules

- Use DESeq2 for raw integer counts, not TPM or FPKM; use a normalized-data method for normalized inputs.
- Four or more replicates per group are recommended; for two or three per group, consider edgeR quasi-likelihood.
- Use condition-only, batch-adjusted, paired, or interaction designs according to the experimental structure, and do not include a batch that is confounded with condition.
- Use shrunken log2 fold changes for ranking and plots and unshrunk results for hypothesis testing.
- Prefer apeglm shrinkage, use ashr when speed is important, and treat normal shrinkage as legacy.
- Use VST for more than 30 samples and rlog for fewer than 30 samples.
- Use adjusted p-values rather than raw p-values for significance decisions.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_05307380030b2d3a` — User raw count matrix and matching sample metadata: Replicated integer-count RNA-seq data are available.

### Secondary resources

- `rr_9931bde2d2a04bd2` — Bioconductor pasilla dataset: Testing or demonstrating the workflow.

### Fallback resources

- `rr_277fc3d07a5c2e61` — limma-voom: Only normalized TPM or FPKM-like expression is available.
- `rr_fd8b64922127e09b` — edgeR quasi-likelihood: Group sizes are only two or three samples.

### Optional resources

- `rr_8a7a79093107ed07` — DESeq2: Count-based differential-expression modeling.
- `rr_398c44e3bc5a4c8d` — apeglm: Recommended log-fold-change shrinkage.
- `rr_5ab4f7dac99a5467` — ashr: Alternative shrinkage method for larger datasets or faster execution.
- `rr_f3a4936deaf955aa` — ggplot2: QC and result visualization.
- `rr_bcb48b51f2cfb767` — tximport: Import Salmon or Kallisto quantification for count-based analysis.
- `rr_042493e33eff103b` — VST: Variance-stabilizing transformation, preferred for larger sample counts.
- `rr_cb2049dbd4086642` — rlog: Regularized-log transformation, preferred for smaller sample counts.
- `rr_63cfe83f880c4d9b` — DESeq2 count model: Differential-expression model for raw RNA-seq counts.

## Validation / QC

- Require count-matrix columns and metadata row names to match.
- Pre-filter low-count genes and set the control or reference level explicitly.
- Check dispersion trend, PCA clustering, MA symmetry, and volcano structure before trusting results.
- Inspect PCA for batch structure and revise the design only when batch and condition are not confounded.
- Document the design formula and DESeq2 version.
- Evidence requirement: Name the coefficient or contrast used and inspect available result names.
- Evidence requirement: Keep shrunken ranking effects separate from unshrunk hypothesis-test results.
- Evidence requirement: Use adjusted p-values and report the chosen significance and effect-size thresholds.
- Evidence requirement: Retain normalized and transformed counts and the fitted DESeqDataSet for downstream checks.

## Failure handling

- Sample identifiers do not match between counts and metadata.
- The design matrix is not full rank because covariates are confounded.
- Counts are non-integer because transcript-level estimates were imported incorrectly.
- Factor names or reference levels do not match the design.
- No genes pass the significance criteria or adjusted p-values are unavailable for low-count genes.
- Fallback rule: Install missing dependencies and retry before adapting the workflow.
- Fallback rule: For transcript quantification input, use the tximport-aware DESeqDataSet constructor rather than coercing values to integers.
- Fallback rule: If SVG export is unavailable, retain the PNG QC outputs.
- Fallback rule: Route normalized TPM or FPKM data to limma-voom and very small groups to edgeR.
- Fallback rule: If dedicated report generation is unavailable, disclose use of the packaged markdown, HTML, or script fallback.

## Limitations

- DESeq2 requires raw count-scale input and is not appropriate for TPM or FPKM matrices.
- Two or three samples per group provide weak support compared with methods designed for very small groups.
- Confounded designs cannot separate condition from batch or other covariates.
- The core workflow does not provide downstream gene annotation, advanced visualization, or functional enrichment.

## Important domain-specific rules

- Enforce a raw-count and sample-metadata contract before differential-expression modeling.
- Choose and document a non-confounded design formula that matches simple, paired, adjusted, or interaction experiments.
- Separate shrunken ranking and visualization effects from unshrunk hypothesis-testing estimates.
- Gate interpretation on dispersion, PCA, MA, and sample-alignment checks.
- Preserve result tables, transformed matrices, and fitted objects for reproducible downstream analysis.

## Portability boundary

- Mandatory skill-local R scripts, relative working-directory assumptions, and exact success-message checks. — migration action: `exclude_or_capability_map`
- Biomni container path examples such as /mnt/results. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation routing and packaged-report fallback. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
