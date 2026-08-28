# Coexpression Network

Source workflow: `coexpression-network`  
Parent Claude Science skill: `regulatory-network-analysis`

## Purpose

Build weighted gene co-expression networks to identify co-expressed modules, module-trait associations, and highly connected hub genes.

## When to use

- Identify gene modules associated with experimental conditions or phenotypes.
- Discover highly connected hub genes within modules.
- Reduce expression data to module eigengenes for downstream analysis.

## Inputs

- Normalized gene-expression matrix with genes as rows and samples as columns. (required)
- Sample metadata with identifiers matching expression-matrix columns and traits for correlation. (required)
- Differential-expression results or gene annotations for optional overlays and enrichment. (optional)

## Outputs

- Gene-to-module assignments with intramodular connectivity and module-membership metrics.
- Ranked hub genes for each module.
- Module-trait correlations, p-values, and module eigengenes.
- Soft-power, dendrogram, trait-correlation, eigengene, and hub-gene figures.
- Reusable network, module-color, expression-matrix, and full-result analysis objects.

## Workflow

1. Confirm normalized expression values, sample count, matching metadata, missingness, and batch correction.
2. Filter to a suitable set of the most variable genes.
3. Select a soft-thresholding power using the scale-free topology diagnostic.
4. Construct the weighted network and detect co-expression modules.
5. Compute module eigengenes and correlate them with the requested traits.
6. Rank hub genes using module membership and intramodular connectivity, then optionally perform functional enrichment.
7. Export tables, figures, and reusable network objects.

## Decision rules

- Require at least 15 samples and prefer 20 or more for robust network inference.
- Do not use raw counts; use VST, rlog, TPM, or FPKM values after correcting batch effects.
- Use 5,000 to 15,000 of the most variable genes as the recommended feature range.
- Do not skip soft-power selection.
- Treat absolute module-trait correlation above 0.5 with p below 0.05 as a significant association in the documented interpretation guide.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_12a726a2c3e2ddd8` — User-supplied normalized expression matrix and matched sample metadata: The study has at least 15 samples and relevant traits.

### Secondary resources

- `rr_c5e6af5a3acdc8fa` — Female mouse liver example dataset: A demonstration or pipeline check is needed.

### Fallback resources

- `rr_2644225052674c6a` — An alternative analysis method: Fewer than 15 samples are available.

### Optional resources

- `rr_cc8f1347f6a35c40` — WGCNA: Weighted-network construction, module detection, eigengenes, and connectivity metrics.
- `rr_f3a4936deaf955aa` — ggplot2: Network diagnostic and summary figures.
- `rr_c8a494b2f458c9b8` — ComplexHeatmap: Module-trait and eigengene heatmaps.
- `rr_a487141e81e96829` — clusterProfiler: Optional module enrichment analysis.
- `rr_1651f8dbdd747818` — Weighted gene co-expression network analysis: Groups genes into modules according to expression-pattern similarity across samples.

## Validation / QC

- Confirm that expression values contain no missing values and that batch effects have been removed or corrected.
- Inspect the scale-free topology fit and data quality when the diagnostic does not reach the recommended range.
- Verify module colors align with gene order before hub-gene calculation.
- Evidence requirement: Report module-trait correlation magnitude and p-value rather than relying on module color alone.
- Evidence requirement: Treat hub genes as candidates for experimental validation, not validated regulators.

## Failure handling

- Fewer than 15 samples produce unstable networks.
- Uncorrected batch effects or poor normalization prevent an acceptable scale-free topology fit.
- Overly large minimum module size or poor filtering assigns most genes to the grey module.
- Too many genes exhaust memory during network construction.
- Fallback rule: Normalize raw counts before analysis and correct batch effects when diagnostics indicate technical structure.
- Fallback rule: Lower the minimum module size or increase the variable-gene set when most genes remain unassigned.
- Fallback rule: Reduce the analysis to 5,000 to 10,000 variable genes when memory is limiting.

## Limitations

- Small datasets with fewer than 15 samples are not suitable for this workflow.
- Co-expression reveals association and generates regulatory hypotheses; hub genes still require experimental validation.

## Important domain-specific rules

- Variable-gene filtering followed by scale-free soft-power selection.
- Module eigengene compression and module-trait correlation.
- Hub ranking from module membership and intramodular connectivity.

## Portability boundary

- Packaged-script-only WGCNA, plotting, and export orchestration with Biomni verification strings. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation packaging and platform-specific path restrictions. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
