# Proteomics Diff Exp

Source workflow: `proteomics-diff-exp`  
Parent Claude Science skill: `proteomics-differential-analysis`

## Purpose

Perform differential protein-expression analysis for TMT or LFQ mass-spectrometry data with limma linear models and DEqMS PSM-count-aware variance correction.

## When to use

- Analyze protein quantification data from TMT or LFQ experiments with biological replicates.
- Apply PSM-aware differential testing when peptide-spectrum-match or peptide counts are available.
- Generate differential-expression tables, quality-control plots, reusable analysis objects, and a scientific report.

## Inputs

- Protein intensity matrix with proteins in rows and samples in columns. (required)
- PSM-level table containing a gene or protein column, or a protein-level intensity table. (required)
- Sample metadata data frame containing a condition column. (required)
- PSM or peptide counts per protein for DEqMS variance correction. (optional)
- Requested condition contrast and adjusted-p-value and fold-change thresholds. (optional)

## Outputs

- Full and threshold-filtered DEqMS result tables with log fold change, PSM-aware p-values, adjusted p-values, and counts.
- Normalized protein-intensity matrix, protein PSM counts, and a top-100 protein table.
- Serialized analysis object containing the fitted model, DEqMS results, protein matrix, metadata, and PSM counts.
- Intensity, missingness, PCA, sample-correlation, volcano, MA, and variance-versus-PSM plots in PNG and SVG formats.
- Markdown analysis report and, when requested, a PDF analysis report.

## Workflow

1. Load example data or user data, then validate the protein matrix, PSM information, and sample metadata.
2. Run the packaged limma-plus-DEqMS workflow for the selected condition contrast.
3. Generate all packaged quality-control and differential-expression plots.
4. Export tables, the normalized matrix, analysis object, and Markdown report.
5. When a PDF is requested, assemble it from the Markdown summary, exported tables, and generated figures, using the packaged fallback only if the reporting capability is unavailable.

## Decision rules

- Use this workflow for TMT or LFQ protein quantification, not RNA-seq, metabolomics, or precomputed fold changes without raw intensities.
- Require at least two biological replicates per condition and prefer at least three.
- Default to adjusted p-value below 0.05 and absolute log2 fold change above 0.58 unless the user selects relaxed or stringent thresholds.
- Use MinProb imputation and median normalization by default; optionally select kNN imputation, quantile normalization, or no normalization.
- Use the packaged analysis, plotting, and export functions and follow the documented script-failure hierarchy before rewriting an analysis.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_13ac9cea76170a1e` — Packaged R scripts for data loading, limma/DEqMS analysis, quality-control plotting, and export.: Use for the standard reproducible workflow and its expected verification messages.
- `rr_050b45df914c27f3` — User-supplied TMT or LFQ protein intensity, PSM, and metadata files.: Use when the user has experimental data in a supported format.

### Secondary resources

- `rr_1532aa0190a3018e` — A431 TMT 10-plex example dataset loaded through ExperimentHub.: Use for a quick demonstration or workflow test when user data are not supplied.
- `rr_a59c64965708c03a` — Detailed method and normalization guidance in the packaged references.: Use when changing comparison, imputation, or normalization choices.

### Fallback resources

- `rr_35b3899e625512cc` — Packaged RMarkdown, HTML, or script reporting fallback.: Use only when the dedicated PDF reporting capability is unavailable, and disclose the fallback.
- `rr_69170c6aae876c9b` — Base R SVG device.: Use automatically when the optional svglite dependency is unavailable or incompatible.

### Optional resources

- `rr_7a50dd7d85c224fe` — limma: Fit linear models for differential protein expression.
- `rr_66d654f7aba59088` — DEqMS: Apply PSM-count-aware variance correction and testing.
- `rr_80550150e807f766` — ExperimentHub: Retrieve the example proteomics dataset.
- `rr_f3a4936deaf955aa` — ggplot2: Create analysis plots.
- `rr_9c40795c5beef2c4` — ggprism: Support plot styling in the packaged workflow.
- `rr_03b6f0ca22cc5563` — ggrepel: Label selected points in plots.
- `rr_c8a494b2f458c9b8` — ComplexHeatmap: Create missingness and sample-correlation heatmaps.
- `rr_77b4f130e7222be2` — circlize: Support ComplexHeatmap color mapping.
- `rr_967681c6238f3918` — matrixStats: Provide matrix summary operations used by the workflow.
- `rr_9ff0dfc885fcb1e2` — rmarkdown and knitr: Provide the optional packaged report fallback.
- `rr_75f9beb4daf6a6e9` — impute and vsn: Provide optional kNN imputation and VSN-related functionality.
- `rr_33b7bc37e29aea5d` — scripts/load_example_data.R: Load example data and validate user inputs.
- `rr_b565ceb1bed3bfb0` — scripts/basic_workflow.R: Run the limma and DEqMS pipeline with aggregation, imputation, and normalization.
- `rr_e454cdea371d4d70` — scripts/qc_plots.R: Generate the standard plot suite.
- `rr_0154d8cebed67248` — scripts/export_results.R: Export CSV, RDS, Markdown, and fallback report artifacts.
- `rr_abbf99b8d8602a79` — pdf-report-generation: Create the requested final PDF from analysis outputs.
- `rr_9c156832df82ce73` — limma linear model: Estimate condition contrasts for protein intensities.
- `rr_a8471a79a7914612` — DEqMS PSM-count-aware variance model: Correct variance estimates using protein PSM counts.

## Validation / QC

- Inspect intensity distributions before and after normalization.
- Inspect sample missingness, PCA separation, and Pearson sample correlations.
- Inspect the DEqMS variance-versus-PSM-count relationship.
- Require the documented verification messages after data loading, analysis, plotting, and export.
- If no proteins are significant, examine PCA and the selected contrast before relaxing thresholds.
- Use biological replication of at least two samples per condition, with three or more recommended.
- Evidence requirement: Report log fold changes, PSM-aware p-values, adjusted p-values, and protein counts in the result tables.
- Evidence requirement: Retain the normalized matrix, PSM counts, metadata, fitted model, and full results in reusable artifacts.
- Evidence requirement: Support analysis conclusions with the standard diagnostic and differential-expression plots.
- Evidence requirement: Build a requested PDF from the Markdown summary, exported result tables, and generated figures, and disclose any fallback report path.

## Failure handling

- The expected verification messages are absent because the workflow was replaced with unverified inline code.
- A script cannot be opened because an unsupported absolute path was used.
- ExperimentHub retrieval times out.
- Required R packages are missing.
- SVG export fails because an optional device is unavailable or incompatible.
- All proteins are removed by an overly stringent missing-value filter.
- No proteins pass significance thresholds because the effect is weak or the contrast is incorrect.
- Fallback rule: On script failure, first install missing dependencies and retry; only then modify the script, adapt it as a cited reference, or write from scratch in that order.
- Fallback rule: Increase the R timeout and retry an ExperimentHub download failure.
- Fallback rule: Use the automatic base R SVG fallback rather than installing svglite when SVG export fails.
- Fallback rule: Adjust the missing-value filter if all proteins are removed.
- Fallback rule: Check PCA and the requested contrast before trying relaxed significance thresholds.
- Fallback rule: Use the packaged report fallback only when PDF report generation is unavailable and disclose that choice.

## Limitations

- The workflow is scoped to TMT or LFQ protein quantification and is not intended for RNA-seq or metabolomics.
- Precomputed fold changes without raw protein intensities are insufficient input.
- PSM or peptide counts are critical to obtain the intended DEqMS variance-correction benefit.
- Two replicates per condition are only a minimum; three or more are recommended.
- A lack of significant proteins can reflect weak effects, a wrong contrast, or threshold choice rather than a pipeline error.

## Important domain-specific rules

- Validation of protein-intensity matrices, PSM information, and condition metadata.
- Explicit imputation and normalization selection with documented defaults.
- Combined limma differential modeling and PSM-count-aware DEqMS variance correction.
- Multi-axis quality control covering distributions, missingness, PCA, correlations, differential plots, and variance-count behavior.
- Stable export of full and filtered tables, normalized data, model objects, plots, and reports.

## Portability boundary

- Mandatory invocation of the Biomni skill package's exact R scripts and platform verification messages. — migration action: `exclude_or_capability_map`
- Delegation of final PDF creation to the Biomni pdf-report-generation skill. — migration action: `exclude_or_capability_map`
- Biomni package-relative script-path rule and prescribed platform script-failure hierarchy. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
