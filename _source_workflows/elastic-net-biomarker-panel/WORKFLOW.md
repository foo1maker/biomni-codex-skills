# Elastic Net Biomarker Panel

Source workflow: `elastic-net-biomarker-panel`  
Parent Claude Science skill: `biomarker-and-signature-analysis`

## Purpose

Select a minimal, interpretable binary-outcome biomarker panel from high-dimensional omics data using elastic-net or LASSO logistic regression, nested cross-validation, stability selection, and optional independent validation.

## When to use

- Discover a sparse biomarker panel for a binary clinical outcome.
- Estimate discovery-cohort discrimination and calibration.
- Validate a locked panel in an independent cohort when one is supplied.

## Inputs

- A genes-by-samples expression or abundance matrix. (required)
- Sample metadata containing a binary outcome with row names matching expression columns. (required)
- At least 20 samples per group; 40 or more per group is recommended. (required)
- An independent validation matrix and metadata with the same outcome. (optional)
- A pre-filtered feature list from differential expression or co-expression analysis. (optional)

## Outputs

- Panel features with coefficients and selection frequencies, plus all-feature stability rankings.
- Held-out discovery predictions and per-fold full-model and locked-panel performance tables.
- External validation metrics when a validation cohort is supplied.
- Parameters, leakage status, model objects, consistency-gate results, and analysis figures.

## Workflow

1. Load the expression matrix and metadata and validate sample and binary-outcome alignment.
2. Prepare features and perform variance filtering and scaling inside cross-validation folds.
3. Fit elastic-net or LASSO logistic models with nested cross-validation and stability selection.
4. Generate discrimination, stability, coefficient, calibration, AUC-distribution, and heatmap outputs.
5. Export machine-readable results and run a consistency gate between reported values and exported files.
6. When available, evaluate the locked panel in an independent cohort before calling it validated.

## Decision rules

- Use elastic net with alpha 0.5 by default to retain correlated features; use alpha 1.0 when the sparsest pure-LASSO panel is desired.
- For more than 5,000 features, prefer the top 2,000 most variable features; alternatives are top 500 or all features.
- Use mean held-out CV AUC from discovery_performance.csv as the discovery performance estimate, not the in-sample final-model ROC AUC.
- Report locked-panel CV AUC alongside full-model CV AUC and label the locked-panel estimate as selection-biased.
- Do not call a discovery-only panel validated and disclose explicitly when external validation was not performed.
- Do not infer panel-gene biology from names alone; require downstream functional enrichment.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_8b6a4a69fa9c2c54` — User expression matrix and binary-outcome metadata: The user has aligned omics data and outcome labels.

### Secondary resources

- `rr_e9076808d73ba0c6` — MARS Consortium sepsis demonstration dataset: A workflow demonstration is requested instead of analysis of user data.

### Fallback resources

- `rr_d6992ae8bb5b438f` — Same-platform validation cohort: Cross-platform validation retains fewer than half of panel features.

### Optional resources

- `rr_8d900a4c285bf949` — NCBI Gene Expression Omnibus: Source accessed through GEOquery for example and validation transcriptomic cohorts.
- `rr_1300c185f3e02f1c` — glmnet: Elastic-net and LASSO penalized logistic regression.
- `rr_43f046376fe4e63a` — pROC: ROC and AUC analysis.
- `rr_c8a494b2f458c9b8` — ComplexHeatmap: Panel-feature heatmap generation.
- `rr_7a50dd7d85c224fe` — limma: Optional differential-expression pre-filtering.
- `rr_d3ddbc0323f129b9` — Elastic-net logistic regression: Recommended sparse classifier with alpha 0.5.
- `rr_7e90b70758803bc3` — LASSO logistic regression: Pure L1 sparse classifier with alpha 1.0.

## Validation / QC

- Require expression columns and metadata row names to match and the outcome to contain exactly two levels.
- Perform variance filtering and scaling within CV folds to prevent train-test leakage.
- Use report_consistency_check.csv to verify that reported AUCs and parameters match exported results.
- Cite leakage_status verbatim from parameters.csv.
- Use independent validation as the unbiased performance estimate when available.
- Evidence requirement: Base discovery performance claims on discovery_performance.csv and identify whether external validation was performed.
- Evidence requirement: Report coefficients and selection frequencies directly; require enrichment evidence before biological interpretation.
- Evidence requirement: Keep full-model CV, locked-panel CV, and independent-validation evidence distinct.

## Failure handling

- Expression and metadata have no shared sample identifiers.
- The outcome is not binary.
- No feature passes the requested stability threshold.
- Cross-platform validation loses most panel features.
- A cross-sectional binary panel is misapplied to longitudinal disease progression.
- Fallback rule: Reduce inner folds or increase sample size when penalized regression does not converge.
- Fallback rule: When no feature passes stability, inspect the automatically relaxed top-feature panel and consider elastic net alpha 0.5.
- Fallback rule: Use a same-platform validation cohort when fewer than half of panel features match across platforms.

## Limitations

- Discovery-cohort mean CV AUC is optimistic and is not independent validation.
- Locked-panel CV is selection-biased because the same data were used to select the panel.
- Cross-sectional binary classification cannot model disease progression or trajectories.
- Panel feature names alone do not establish biological mechanism.

## Important domain-specific rules

- Run preprocessing inside resampling folds and preserve leakage status as an exported artifact.
- Combine nested cross-validation with stability selection for sparse-panel discovery.
- Separate discovery, selection-biased locked-panel, and independent-validation estimates in every report.
- Require functional-enrichment evidence before interpreting selected features biologically.

## Portability boundary

- Mandatory Biomni package scripts and exact source() function names. — migration action: `exclude_or_capability_map`
- Biomni demonstration loaders and fixed example-dataset routing. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation and GenerateImage terminal step. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
