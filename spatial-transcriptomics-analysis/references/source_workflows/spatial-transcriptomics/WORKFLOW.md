# Spatial Transcriptomics

Source workflow: `spatial-transcriptomics`  
Parent Claude Science skill: `spatial-transcriptomics-analysis`

## Purpose

Analyze 10x Visium spatial gene-expression data through quality control, spatial-domain clustering, spatially variable gene detection, and neighborhood analysis.

## When to use

- Identify spatially variable genes across a tissue section.
- Discover spatial tissue domains through clustering.
- Quantify neighborhood enrichment and distance-dependent co-occurrence between clusters.

## Inputs

- 10x Visium data in AnnData, HDF5, or Space Ranger output-directory format with expression and spatial coordinates. (required)
- An embedded histology image for spatial overlays. (optional)
- Clustering resolution, mitochondrial-content threshold, and marker genes for user-provided data. (optional)

## Outputs

- A processed AnnData object containing clusters, embeddings, spatially variable gene results, and a spatial graph.
- Tables for spatially variable genes, cluster assignments, neighborhood enrichment, spot metadata, and an analysis summary.
- Quality-control, spatial-cluster, marker, embedding, neighborhood, co-occurrence, and spatially variable gene figures.

## Workflow

1. Confirm whether the user will provide Visium data or use the built-in V1_Human_Heart example.
2. Load the expression matrix, spatial coordinates, and optional histology image.
3. Run preprocessing, spatial graph construction, clustering, spatially variable gene analysis, neighborhood enrichment, and co-occurrence analysis.
4. Generate both spatial and embedding-based visualizations, using tissue-appropriate marker genes when supplied.
5. Export the processed analysis object, tables, figures, and human-readable summary.

## Decision rules

- Use this workflow for 10x Visium data with spatial coordinates, not single-molecule imaging platforms or non-spatial single-cell data.
- Use a higher mitochondrial-content threshold for mitochondria-rich cardiac or muscle tissue and a lower threshold for other tissues.
- Interpret Moran's I and neighborhood scores with their statistical thresholds and the tissue context.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_df3aa100a435687c` — User-provided 10x Visium data: The user has an AnnData object, HDF5 file, or Space Ranger output directory to analyze.

### Secondary resources

- `rr_0df04e72b5b29e62` — 10x Genomics V1_Human_Heart example: The user selects the built-in example instead of supplying data.

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- `rr_eec08a2fce1f27c1` — Squidpy: Spatial graph, neighborhood, co-occurrence, and spatial-variable analysis.
- `rr_063ec900fce6482b` — scanpy: AnnData preprocessing, embeddings, and clustering support.
- `rr_6aba97b6dceeffd7` — scikit-learn: General machine-learning support in the analysis environment.
- `rr_42bc4f5de0dd382f` — matplotlib and seaborn: Scientific plotting.

## Validation / QC

- Verify that spatial coordinates are present before spatial analysis.
- Report spots analyzed, clusters found, and the number of significant spatially variable genes.
- Compare spatial clusters with histology when an image is available.
- Evidence requirement: Report Moran's I values and false-discovery-rate thresholds for spatially variable genes.
- Evidence requirement: Do not assign gene functions without known annotations or a separate lookup.

## Failure handling

- Spatial coordinates are absent from the input.
- No spatially variable genes pass the selected false-discovery-rate threshold.
- Large spot-by-gene matrices exceed available memory.
- Co-occurrence estimates contain missing values under an unsupported split configuration.
- Fallback rule: Use generic coordinate handling for non-grid spatial data.
- Fallback rule: When no spatially variable genes pass the default threshold, increase permutations or relax the false-discovery-rate threshold and disclose the change.
- Fallback rule: When SVG export is unavailable, retain PNG output and disclose that SVG was best-effort.

## Limitations

- Permutation count affects spatially variable gene p-values and can reduce sensitivity to weak signals.
- Co-occurrence curves should not be over-interpreted for small datasets or very few clusters.
- This workflow is scoped to 10x Visium and not to single-molecule imaging platforms.

## Important domain-specific rules

- Validate spatial coordinates, expression data, and tissue-specific quality thresholds before analysis.
- Combine clustering, spatially variable gene statistics, neighborhood enrichment, and distance-dependent co-occurrence in one analysis.
- Cross-check inferred spatial domains against tissue histology when available.

## Portability boundary

- Mandatory use of bundled skill-local loader, workflow, plotting, and export scripts with exact success tokens. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation invocation and report packaging fallback. — migration action: `exclude_or_capability_map`
- Phylo-specific figure and report presentation conventions. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
