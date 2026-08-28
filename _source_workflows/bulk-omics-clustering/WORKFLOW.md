# Bulk Omics Clustering

Source workflow: `bulk-omics-clustering`  
Parent Claude Science skill: `bulk-rna-and-omics-analysis`

## Purpose

Cluster biological samples or features in normalized quantitative matrices, compare method assumptions, and validate cluster separation and stability.

## When to use

- Group bulk-omics samples into data-driven subtypes.
- Cluster features by shared quantitative patterns.
- Detect outliers or batch structure and compare clustering approaches.

## Inputs

- A quantitative matrix in CSV, TSV, Excel, HDF5, or RDS format with declared row and column orientation. (required)
- Normalized and comparable values with missing values imputed or removed. (required)
- Sample or feature metadata with matching identifiers. (optional)
- At least 10 samples or features; 20 or more is recommended. (required)

## Outputs

- Cluster assignments, cluster sizes, centroids, and characteristics.
- Silhouette, Davies-Bouldin, Calinski-Harabasz, and optional bootstrap-stability metrics.
- Cluster-distinguishing feature tables.
- Serialized clustering object and the normalized matrix used for analysis.
- Dendrogram, PCA or UMAP, silhouette, heatmap, cluster-size, and optimal-k figures.

## Workflow

1. Load the matrix, confirm orientation and identifiers, normalize or scale appropriately, handle missing values, and address batches and extreme outliers.
2. Choose clustering direction, algorithm, distance metric, and candidate cluster count from the data and analysis goal.
3. Fit hierarchical, k-means, HDBSCAN, or model-based clustering as appropriate.
4. Validate assignments with internal metrics, known labels when available, bootstrap stability, and biological or technical plausibility.
5. Generate dimensionality-reduction views, heatmaps, silhouettes, and other method-appropriate figures, then export assignments, metrics, parameters, and fitted objects.

## Decision rules

- Prefer hierarchical clustering for exploratory analyses below about 5,000 samples, k-means for speed above about 5,000 with compact clusters, HDBSCAN for unknown k or outliers, and GMM when soft memberships are needed.
- Use correlation distance for expression patterns, Euclidean for normalized continuous data, Manhattan for outlier robustness, and cosine for sparse high-dimensional data.
- For unknown k, compare a range such as 2–15 with multiple metrics rather than selecting k from a single plot.
- For more than 1,000 features, consider PCA before clustering; use UMAP primarily for visualization.
- Remove or regress batch effects and investigate extreme outliers before clustering.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_768976ec1e5ecb26` — User-provided normalized quantitative matrix: A biological sample-by-feature or feature-by-sample matrix is available.

### Secondary resources

- `rr_1c6a1f0c3a7d3130` — Bioconductor ALL demonstration dataset: No user data are available or the workflow needs validation.

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- `rr_5bd73a390b9bdcf4` — Bioconductor ALL dataset: Mixed-age acute lymphoblastic leukemia demonstration and workflow-validation fixture.
- `rr_6aba97b6dceeffd7` — scikit-learn: Python clustering, dimensionality reduction, and validation metrics.
- `rr_ff66d8a2831b3a9d` — hdbscan: Density-based clustering with noise detection.
- `rr_0c9c2cb673788e2c` — umap-learn: Optional manifold visualization.
- `rr_c8a494b2f458c9b8` — ComplexHeatmap: R heatmap visualization.
- `rr_9b3fd110fcf1354e` — mclust: R model-based clustering.
- `rr_b1a42a2762ca4830` — PCA: Dimensionality reduction before clustering and for visualization.
- `rr_9844b0bbbbc9a93d` — UMAP: Nonlinear visualization of cluster structure.
- `rr_a9bbd58ad0665042` — Silhouette score: Internal cohesion and separation metric.
- `rr_d0240296f3e47c1f` — Davies-Bouldin index: Internal cluster-separation metric.
- `rr_d828e130c1221d56` — Calinski-Harabasz score: Internal variance-ratio validation metric.
- `rr_20ba9bc2947a7e74` — Hierarchical clustering: Deterministic multiscale clustering for exploratory analysis.
- `rr_e33af3ad72790ee1` — K-means: Fast partitioning for compact clusters with a specified k.
- `rr_fd29888cae9231fa` — HDBSCAN: Density-based clustering with automatic cluster count and outlier labels.
- `rr_24249a7f90c175ab` — Gaussian mixture model: Probabilistic soft clustering for overlapping groups.

## Validation / QC

- Confirm matrix orientation, identifier alignment, normalization, missing-value handling, batch correction, and outlier review.
- Report multiple internal metrics and bootstrap stability rather than relying on a single score.
- Use known labels or biological validation when available without treating them as clustering inputs.
- Inspect PCA or UMAP views, cluster sizes, noise labels, and run-to-run stability.
- Evidence requirement: Retain assignments, parameters, validation metrics, fitted objects, and the exact normalized matrix used.
- Evidence requirement: Document the algorithm, distance, k-selection rationale, and validation evidence.

## Failure handling

- The requested cluster count exceeds the number of observations.
- Ward linkage is paired with a non-Euclidean distance.
- Input contains missing values or unhandled batch effects.
- HDBSCAN labels every observation as noise.
- K-means assignments change substantially across random initializations.
- Silhouette score indicates weak separation.
- Fallback rule: For all-noise HDBSCAN results, reduce minimum cluster size or adjust minimum samples.
- Fallback rule: For unstable k-means, increase n_init or use hierarchical clustering.
- Fallback rule: Impute or remove missing values before rerunning clustering.
- Fallback rule: When optional UMAP or visualization dependencies conflict, restore a compatible environment or omit that optional view while retaining core clustering outputs.
- Fallback rule: If dedicated report generation is unavailable, disclose use of the packaged markdown, HTML, or script fallback.

## Limitations

- The workflow is not intended for single-cell clustering or gene co-expression-network inference.
- Raw or non-comparable values are unsuitable until appropriately normalized.
- Each algorithm assumes particular cluster shapes, densities, or a specified number of clusters.
- Very small matrices provide weak support for robust cluster discovery.

## Important domain-specific rules

- Make matrix orientation, normalization, missingness, batches, and outliers explicit before clustering.
- Choose algorithm, distance, and k from declared assumptions and the data geometry.
- Triangulate cluster quality with internal metrics, stability, known labels, and domain plausibility.
- Preserve assignments, parameters, fitted objects, and the analyzed matrix for reproducibility.

## Portability boundary

- Mandatory skill-local Python or R scripts and exact success-message checks. — migration action: `exclude_or_capability_map`
- Biomni-specific ALL example loader and rpy2 routing. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation routing and packaged-report fallback. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
