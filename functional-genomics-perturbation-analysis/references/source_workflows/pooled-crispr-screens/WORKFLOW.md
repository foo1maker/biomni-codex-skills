# Pooled Crispr Screens

Source workflow: `pooled-crispr-screens`  
Parent Claude Science skill: `functional-genomics-perturbation-analysis`

## Purpose

Analyze pooled CRISPR screens with single-cell RNA-seq readout using a tiered workflow of fast screening, perturbation-target validation, and rigorous differential expression.

## When to use

- Analyze Perturb-seq, CROP-seq, CRISPRi, CRISPRa, or CRISPR knockout screens with scRNA-seq readout.
- Process 10X Feature Barcoding data with captured sgRNA feature barcodes.
- Combine biological-replicate libraries and identify perturbations with transcriptional phenotypes.

## Inputs

- One 10X Feature-Barcode H5 matrix per library containing gene-expression and sgRNA counts. (required)
- One sgRNA-to-cell mapping file per library with cell barcode and sgRNA identifier. (required)
- sgRNA identifiers containing target gene names or a separate gene lookup table. (required)
- Screen type, replicate structure, controls, cell type, expected direction, and analysis goals for user-supplied data. (optional)

## Outputs

- Fast per-perturbation screening statistics, perturbation validation results, and a final validated hit list.
- Batch-corrected glmGamPoi differential-expression results for selected top hits when the optional R dependency is available.
- Normalized and fully annotated H5AD objects plus a pickle of perturbation summaries, differential-expression results, and outlier cells.
- QC metrics, volcano plots, and target-expression heatmaps in PNG and SVG.
- Publication-oriented CRISPR screen report, with a text report available even when optional PDF support is unavailable.

## Workflow

1. Load each 10X library, map sgRNAs to cells, apply cell-type-appropriate QC, and concatenate libraries with batch labels.
2. Correct gene names, filter genes by expression within perturbation groups, and normalize counts.
3. Run a fast t-test screen against non-targeting controls and call preliminary hits by the number of differentially expressed genes.
4. Validate that each perturbation changes its target in the expected CRISPRi or CRISPRa direction, and retain only preliminary hits that pass.
5. Optionally run batch-corrected glmGamPoi differential expression for the highest-priority validated hits.
6. Generate perturbation visualizations and export tables, H5AD, pickle, hit lists, and reports.

## Decision rules

- Do not use this workflow for arrayed screens, non-transcriptional readouts, or data without sgRNA assignments.
- Ask about input files first; when the real example dataset is selected, use its predefined parameters and skip later clarification questions.
- Use expected direction down for CRISPRi and up for CRISPRa when validating target effects.
- Call preliminary hits after fast screening, then require target-effect validation before treating them as validated hits.
- Reserve the rigorous glmGamPoi stage for top validated hits and skip it when the optional R dependency is unavailable.
- Use cell-type-specific QC thresholds rather than one fixed threshold set.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_59fc2bb7d04ddd6a` — User-supplied 10X Feature-Barcode matrices and sgRNA mapping files: The user has a mapped pooled single-cell CRISPR screen.
- `rr_ccd870c121549384` — Non-targeting controls and biological-replicate batch labels: Screening and batch-aware differential expression are performed.

### Secondary resources

- `rr_a6623463e8b44ae9` — Papalexi and Satija 2021 ECCITE-seq CRISPRi dataset: A real-data demonstration or pipeline validation is requested.
- `rr_2a4a489ddaa02337` — glmGamPoi: Rigorous batch-corrected differential expression is requested for top validated hits.

### Fallback resources

- `rr_4b4fa64db0ea99cb` — Small synthetic demo dataset: Offline smoke testing is required.
- `rr_274f6ad51aeb70f8` — CellRanger or a CROP-seq assignment pipeline: The data do not yet contain sgRNA-to-cell assignments.
- `rr_79522b1ce7e1f4d3` — Fast t-test screening results: glmGamPoi cannot run because R or the package is unavailable.

### Optional resources

- `rr_09c420e41332986a` — scanpy and anndata: Single-cell data loading, representation, and processing.
- `rr_d4bbf183d34fba1b` — pandas, NumPy, and SciPy: Tabular and numerical processing.
- `rr_6aba97b6dceeffd7` — scikit-learn: Outlier detection.
- `rr_5a15ae9290e2ae11` — diffxpy: Differential-expression support.
- `rr_c9e835f6d9fccde6` — glmGamPoi with rpy2: Optional rigorous batch-corrected differential expression.
- `rr_42bc4f5de0dd382f` — matplotlib and seaborn: QC and perturbation visualizations.
- `rr_96a2486bca14d066` — scripts/load_10x_libraries.py, map_sgrna_to_cells.py, qc_filtering.py, and concatenate_libraries.py: Load, map, QC, and combine libraries.
- `rr_e65dbccc5ca1e50f` — scripts/gene_name_corrections.py, expression_filtering.py, and normalize_and_scale.py: Prepare the expression matrix.
- `rr_83365e687097b5e7` — scripts/screen_all_perturbations.py and validate_perturbations.py: Fast screening, hit calling, and target-effect validation.
- `rr_ac8fb4fafbffb74e` — scripts/differential_expression_glmgampoi.py: Optional rigorous batch-aware differential expression.
- `rr_cc60e3018db2d07b` — scripts/visualize_perturbations.py and export_results.py: Generate plots and export all result artifacts.
- `rr_70fc973e0c41bcfc` — Per-perturbation t-test screen: Fast preliminary screening against non-targeting controls.
- `rr_f5b0bb66773f287a` — glmGamPoi differential-expression model: Rigorous batch-corrected analysis for selected validated hits.
- `rr_bdb126daa3d8d506` — Target-effect validation rule: Confirms the target changes in the direction expected for the screen type.

## Validation / QC

- Require mapping files filtered to single-sgRNA assignments, with doublets removed.
- Use cell-type-specific minimum genes, minimum counts, and maximum mitochondrial-fraction thresholds.
- Track sgRNA mapping retention for every library and verify the combined cell count.
- Validate the target gene effect before promoting a preliminary hit.
- Investigate mapping below 30%, doublets above 10%, validation below 50%, and inconsistent replicate behavior.
- Evidence requirement: Retain both fast-screening and rigorous differential-expression outputs so the basis for each hit is traceable.
- Evidence requirement: Report the number of perturbations screened and the number of validated hits.
- Evidence requirement: Use non-targeting controls, replicate labels, and observed target-expression changes to support hit calls.

## Failure handling

- sgRNA mapping is below 30% because capture efficiency is poor or the mapping file is wrong.
- Doublets exceed 10% because of high multiplicity of infection or barcode collisions.
- Target expression does not change because knockdown is incomplete, the guide is ineffective, or compensation occurs.
- Replicates disagree because of batch effects or population drift.
- Large datasets exhaust memory.
- glmGamPoi cannot run because R or the package is absent.
- Fallback rule: For memory errors, process libraries separately or use backed H5AD mode.
- Fallback rule: For inconsistent replicates, inspect batch balance, consider batch correction, and analyze replicates separately first.
- Fallback rule: Skip glmGamPoi and retain t-test results when the optional R dependency is unavailable.
- Fallback rule: Allow automatic SVG fallback while still attempting both PNG and SVG.

## Limitations

- The workflow is limited to pooled screens with single-cell transcriptional readout.
- Arrayed screens and non-transcriptional readouts are outside scope.
- Data without sgRNA-to-cell assignments require an upstream assignment pipeline.
- Rigorous glmGamPoi analysis is applied only to selected top hits and depends on optional R support.

## Important domain-specific rules

- Tiered fast screen, target-effect validation, and rigorous top-hit differential expression.
- Replicate-aware multi-library loading with cell-type-specific QC and non-targeting controls.
- Portable H5AD, pickle, hit-list, and per-perturbation result artifacts for downstream analyses.

## Portability boundary

- Low-freedom packaged-script execution and exact Biomni verification-message contract. — migration action: `exclude_or_capability_map`
- Packaged report generator and Biomni-oriented downstream skill routing. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
