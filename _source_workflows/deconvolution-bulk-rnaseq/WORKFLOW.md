# Deconvolution Bulk Rnaseq

Source workflow: `deconvolution-bulk-rnaseq`  
Parent Claude Science skill: `bulk-rna-and-omics-analysis`

## Purpose

Estimate cell-type proportions in bulk RNA-seq samples from an annotated single-cell reference and compare composition across groups or timepoints.

## When to use

- Estimate cell-type fractions in bulk RNA-seq samples.
- Compare cell-type composition between groups and across longitudinal timepoints.
- Assess cross-method concordance and identify method-fragile cell types.

## Inputs

- Bulk gene-expression matrix with genes as rows and samples as columns, using gene identifiers compatible with the reference. (required)
- Annotated single-cell reference in AnnData, Seurat, or SingleCellExperiment form with a cell-type column. (required)
- Sample metadata containing sample identifier, group, and optional timepoint and subject identifier. (optional)

## Outputs

- Per-method and cross-method consensus cell-type proportion tables.
- Method-concordance, group-composition, contrast, and optional mixed-model result tables.
- Composition, trajectory, concordance, and optional ground-truth recovery figures.
- A reusable analysis object containing proportions, consensus, concordance, contrasts, and metadata.
- A markdown analysis summary and optional PDF report.

## Workflow

1. Confirm the bulk matrix, single-cell reference, gene-identifier compatibility, expression scale, and sample metadata.
2. Load the reference and bulk data, converting log-scale bulk values to linear scale when required.
3. Select the deconvolution panel according to reference donor structure and analysis needs.
4. Estimate proportions with the selected methods and compute a cross-method consensus.
5. Calculate group or longitudinal composition contrasts with multiple-testing correction.
6. Evaluate method concordance and mark cell types whose estimates are unstable across methods.
7. Generate composition and concordance visualizations and export all tables and the reusable result object.
8. For publication-grade use, validate against independent experimental proportions or independent pseudobulk truth.

## Decision rules

- Use BayesPrism plus DWLS as the recommended default method panel.
- Add MuSiC and BisqueRNA only when the reference contains at least three donors.
- Use BayesPrism alone when the fastest single-method workflow is requested.
- Never use CIBERSORTx, EPIC, or BSeq-sc in this commercially licensed workflow.
- Treat a cell type absent from the reference as a critical failure that can bias every method.
- Pass explicit group levels when a grouping variable has more than two levels.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_01dcac0894926d57` — User-supplied annotated single-cell reference matched to the bulk tissue and gene identifiers: A representative reference with cell-type labels is available.

### Secondary resources

- `rr_6864c784f9123a4e` — CELLxGENE Census: A suitable annotated reference must be constructed from a public census slice.

### Fallback resources

- `rr_d224696436f021a6` — Bundled synthetic immune cohort with known proportions: A demonstration or integration check is requested.

### Optional resources

- `rr_a2e61f6310532050` — BayesPrism: Default reference-based cell-type deconvolution method.
- `rr_01b987660eb7af31` — DWLS: Default deconvolution method designed to handle collinear cell types.
- `rr_98c139728f0e5b78` — MuSiC: Optional multi-donor reference deconvolution method.
- `rr_2a2cde9d0f3646a1` — BisqueRNA: Optional reference-based deconvolution method.
- `rr_e81b06448ef1b2c8` — SingleCellExperiment: Supported reference container.
- `rr_6c0dca6ff0e17cd8` — zellkonverter: AnnData h5ad input support.
- `rr_7dadc66200d982df` — Seurat: Supported RDS reference container.
- `rr_95ec32fb39850ae1` — lmerTest: Optional longitudinal mixed-effects testing.
- `rr_f186fc13ff7b9e6c` — SimBu: Synthetic pseudobulk evaluation only.
- `rr_fb444e64323fafa9` — BayesPrism: Estimates cell-type fractions from bulk expression and a single-cell reference.
- `rr_6cc0a090d47d9b3b` — DWLS: Estimates composition while handling collinearity among related reference cell types.
- `rr_e2aa541e64a976b4` — MuSiC: Uses multi-subject single-cell references for bulk deconvolution.
- `rr_c5e6a69670a7b4dd` — BisqueRNA: Reference-based composition estimation that depends on informative inter-donor variation.

## Validation / QC

- Confirm that bulk and reference use the same gene identifier system and that the bulk scale is interpreted correctly.
- Check that every expected bulk cell type is represented in the reference.
- Verify per-sample proportion rows sum to approximately one.
- Report pairwise and per-cell-type method concordance and flag estimates that flip across methods.
- Use independent truth rather than the same synthetic reference for publication-grade validation.
- Evidence requirement: Label bundled synthetic recovery as a pipeline self-consistency check, not real-world accuracy validation.
- Evidence requirement: For real accuracy claims, compare estimates with orthogonal proportions or independent pseudobulk from the same tissue.
- Evidence requirement: Keep method-specific estimates and concordance available alongside the consensus.

## Failure handling

- The bulk contains a cell type absent from the reference.
- Bulk values are treated on the wrong scale or gene identifiers do not match the reference.
- Closely related cell types produce discordant method estimates.
- BisqueRNA returns near-uniform fractions when donor compositions have insufficient variation.
- Fallback rule: Convert both bulk and reference to matching gene symbols when the shared-gene count is low.
- Fallback rule: Use DWLS and report a method-fragile flag when related cell types are collinear.
- Fallback rule: Retain Wilcoxon testing with BH-FDR if longitudinal mixed modeling is unavailable.
- Fallback rule: Use BayesPrism plus DWLS when optional multi-donor methods are not appropriate.

## Limitations

- Reference-free deconvolution and spatial spot deconvolution are outside scope.
- Results are biased when the reference omits cell populations or differs substantially in tissue, donor, or platform.
- Bundled tests do not establish absolute accuracy on real cohorts.
- CIBERSORTx, EPIC, and BSeq-sc are excluded because their licenses are not compatible with the documented commercial workflow.

## Important domain-specific rules

- Reference and bulk scale/identifier compatibility gate.
- Multi-method deconvolution with consensus proportions and per-cell-type concordance.
- Group and longitudinal composition contrasts with FDR correction.
- Independent ground-truth recovery assessment for publication-grade use.

## Portability boundary

- Packaged R and Python script orchestration, verification messages, and internal subprocess wrapper. — migration action: `exclude_or_capability_map`
- Biomni results-panel paths, S3-FUSE staging behavior, and /mnt/results output conventions. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation packaging and platform report styling. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
