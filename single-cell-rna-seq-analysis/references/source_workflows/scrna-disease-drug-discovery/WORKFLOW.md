# Scrna Disease Drug Discovery

Source workflow: `scrna-disease-drug-discovery`  
Parent Claude Science skill: `single-cell-rna-seq-analysis`

## Purpose

Identify and prioritize disease-relevant drug targets from single-cell RNA-seq by integrating cell-type-resolved differential expression, pathways, ligand-receptor interactions, genetic evidence, and druggability.

## When to use

- Disease-versus-control target discovery, cell-type-resolved differential expression and pathway analysis, ligand-receptor analysis, genetic-evidence integration, and druggability-based prioritization.

## Inputs

- Annotated AnnData object with cell type, condition, and sample or donor identifiers. (required)
- Alternatively, a raw or filtered 10X, H5, CSV, or TSV count matrix for preprocessing. (optional)
- Disease name and species, with human as the stated default. (optional)

## Outputs

- Ranked target table and evidence cards containing transcriptomic, pathway, ligand-receptor, genetic, cell-specificity, and druggability evidence.
- Per-cell-type differential-expression, pathway, ligand-receptor, and genetic-evidence tables; analyzed AnnData; manifest; figures; and report.

## Workflow

1. Load and validate the count matrix and metadata, preserve a checkpoint, and preprocess raw data only when an annotated object is unavailable.
2. Run per-cell-type pseudobulk differential expression, pathway enrichment, and ligand-receptor analysis.
3. Collect candidate genes using stated adjusted-p-value and effect-size thresholds and query genetic-evidence sources.
4. Compute the multi-evidence target score, assign priority tiers, generate the GWAS gene landscape separately, and export tables, figures, and evidence cards.

## Decision rules

- Prefer an annotated AnnData object; preprocess a raw matrix only when necessary, and retain all biological samples.
- Use the counts layer when the main AnnData matrix is log-normalized, and subsample within cell type and condition for datasets larger than 50,000 cells.
- Use Open Targets GWAS and ClinVar as primary genetic evidence, GeneBass when a relevant UK Biobank disease exists, and TWAS or eQTL as supplementary evidence.
- Assign HIGH priority at a score of at least 0.55, MEDIUM from 0.35 to below 0.55, and LOW below 0.35.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_076dd88ccdae5774` — Annotated AnnData object: Use when cell types, conditions, and sample identifiers have already been established.

### Secondary resources

- `rr_7a11ed62e6e65a30` — Raw or filtered count matrix: Use with the included preprocessing path when an annotated object is unavailable.

### Fallback resources

- `rr_360f68eacfa510d7` — Cell-level Wilcoxon and Open Targets-only genetic evidence: Use only as an explicitly exploratory fallback when pseudobulk replication is insufficient or GeneBass is unavailable.

### Optional resources

- `rr_f110aa187bc159dd` — Open Targets GWAS and ClinVar: Primary common-variant and clinical-variant genetic evidence.
- `rr_68cf2c6ee6caf3de` — GeneBass: Rare-variant burden evidence.
- `rr_e112dd3c68e2f5e0` — TWAS Atlas and eQTL resources: Supplementary gene-regulatory genetic evidence.
- `rr_09c420e41332986a` — scanpy and anndata: Single-cell object processing and persistence.
- `rr_0227d39e27cec7e1` — PyDESeq2: Per-cell-type pseudobulk differential expression.
- `rr_d193957023178821` — decoupler, LIANA, and GSEApy: Pathway, ligand-receptor, and enrichment analysis.
- `rr_bb913bba27dc4b84` — Adaptive multi-omics target score: Combines differential expression, pathway centrality, ligand-receptor evidence, cell-type specificity, genetic evidence, and druggability with a convergence bonus and adaptive reweighting.

## Validation / QC

- Require cell-type, condition, and sample metadata, preserve all samples, and checkpoint the input object immediately.
- Read every quantitative result from saved tables rather than reconstructing values from memory.
- Evidence requirement: Prioritize targets using convergent transcriptomic and genetic evidence and retain separate columns for each evidence class and druggability.
- Evidence requirement: Use adjusted p below 0.05 and absolute log2 fold change above 0.25 for candidate-gene collection, and report the GWAS gene landscape separately.

## Failure handling

- Insufficient replicates or degenerate count structure prevents valid pseudobulk DESeq2 analysis and can invalidate downstream enrichment.
- Ligand-receptor analysis may yield no significant interactions or run slowly; external genetic resources may be unavailable or rate-limited.
- Fallback rule: Use cell-level Wilcoxon only as an exploratory fallback when pseudobulk replication is unavailable, and skip GSEA for failed cell types.
- Fallback rule: Use Open Targets when GeneBass is unavailable, retry TWAS with backoff, and redistribute scoring weights when an evidence class is unavailable.

## Limitations

- The workflow is not for annotation-only, genetic-only, gene-regulatory-network-only, or spatial analyses.
- Pseudobulk inference requires replicated samples; large datasets may require stratified subsampling, and external evidence coverage depends on API availability.

## Important domain-specific rules

- AnnData intake, pseudobulk DE, pathway and ligand-receptor evidence, genetic-evidence aggregation, adaptive target scoring, priority tiers, and evidence-card export.

## Portability boundary

- Biomni pdf-report-generation, internal script and evaluation paths, platform-specific report orchestration, and named upstream or downstream Biomni skills. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
