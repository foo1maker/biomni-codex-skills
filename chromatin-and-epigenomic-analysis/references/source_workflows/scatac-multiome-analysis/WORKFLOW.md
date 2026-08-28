# Scatac Multiome Analysis

Source workflow: `scatac-multiome-analysis`  
Parent Claude Science skill: `chromatin-and-epigenomic-analysis`

## Purpose

Analyze fragments-first single-cell chromatin-accessibility data from QC through LSI, clustering, per-cluster peak recall, gene-activity annotation, and differential accessibility.

## When to use

- Ingest indexed fragment files, perform chromatin-accessibility QC and dimensionality reduction, cluster cells, recall peaks per cluster, annotate from gene activity, and test differential accessibility.

## Inputs

- bgzip-compressed fragment file and matching Tabix index. (required)
- Peak-by-barcode H5 or matrix files, or a combined 10X multiome feature-barcode H5. (optional)
- Genome build, limited in the source to hg38, hg19, or mm10, and optional Cell Ranger single-cell metadata. (required)

## Outputs

- QC, peak, cell-type, and differential-accessibility tables; PNG and SVG figures; checkpoints; optional final object; and a provenance manifest.
- Analysis report.

## Workflow

1. Ingest fragments, seed cells from an available matrix or call them from fragment counts, and calculate TSS enrichment, nucleosome signal, FRiP, and blacklist ratio.
2. Run TF-IDF and LSI, discard the depth-associated first component, cluster cells, and compute UMAP.
3. Recall peaks separately for each cluster, remove blacklist regions, retain standard chromosomes, and rebuild the peak matrix.
4. Compute gene activity for a marker panel, assign confidence-gated annotations, optionally transfer labels, and test differential accessibility.
5. Export figures, tables, checkpoints, final object if requested, and reproducibility metadata.

## Decision rules

- Choose the matching EnsDb, BSgenome, and blacklist for the fragment alignment build.
- Always discard LSI component 1 because it is dominated by sequencing depth.
- Recall peaks by cluster rather than from the aggregate cell population.
- Prioritize user markers, then a tissue-matched CellMarker panel, then the built-in fallback, and gate weak or ambiguous annotations by confidence.
- Use Wilcoxon differential-accessibility testing rather than a latent-variable logistic-regression test.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_b2492bfa090a7ef6` — Indexed fragment file: Use as the analytical primitive for QC, cell calling, peak recall, and accessibility analysis.

### Secondary resources

- `rr_2b935ceb6b0c2229` — Peak-by-barcode or multiome feature-barcode matrix: Use for initial cell seeding and QC comparison when available.

### Fallback resources

- `rr_db1d81df1ef89cc9` — Fragment-derived cell calling, peak recall, and QC: Use when matrix or Cell Ranger single-cell metadata is absent.

### Optional resources

- `rr_4311b989dc04833f` — CellMarker 2.0: Tissue-adaptive human or mouse marker panels for accessibility-based annotation.
- `rr_d1f563deccf3d66b` — Signac and Seurat: Single-cell chromatin-accessibility analysis and object handling.
- `rr_af414d8d4eff5eed` — MACS3: Per-cluster peak calling.
- `rr_8384336387e4857b` — EnsDb and BSgenome packages: Genome-build-specific annotation and sequence resources.
- `rr_198c24fdfddaa2e0` — TF-IDF and latent semantic indexing: Dimensionality reduction for sparse chromatin-accessibility data.
- `rr_f3f633986e80d219` — Seurat anchor label transfer: Optional annotation transfer.

## Validation / QC

- Calculate TSS enrichment, nucleosome signal, FRiP, blacklist ratio, and depth correlation; remove blacklist regions and non-standard chromosomes.
- Retain confidence flags and mark weak or ambiguous cell-type annotations.
- Evidence requirement: Retain a provenance manifest containing parameters, package versions, session information, genome build, accession, and random seeds.
- Evidence requirement: Verify that the selected genome resources match the alignment build and qualify accessibility-based annotations by confidence.

## Failure handling

- Genome-build mismatch, retaining LSI component 1, or aggregate peak calling distorts downstream analysis.
- All-gene activity calculation, latent-variable logistic regression, weak markers, or an incorrect tissue marker panel causes excessive compute or unreliable annotation.
- Fallback rule: Call cells and derive FRiP or blacklist metrics from fragments when auxiliary matrix or Cell Ranger metadata is unavailable.
- Fallback rule: Use a built-in marker panel when user-supplied and CellMarker panels are unavailable, and keep label transfer optional.

## Limitations

- The workflow does not align raw reads or generate fragment files and excludes motif or chromVAR analysis, peak-to-gene linkage, trajectory inference, and cross-sample batch integration.
- Accessibility-based annotation is noisier than expression-based annotation, and the captured workflow is single-sample.

## Important domain-specific rules

- Fragments-first QC, TF-IDF and LSI with component-1 exclusion, per-cluster MACS3 recall, marker-panel gene activity, confidence-gated annotation, and Wilcoxon differential accessibility.

## Portability boundary

- Biomni data-lake paths, pdf-report-generation, LiteratureSearch, internal result-directory variable, packaged script orchestration, and Phylo branding. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
