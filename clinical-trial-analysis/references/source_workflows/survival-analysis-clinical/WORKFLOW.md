# Survival Analysis Clinical

Source workflow: `survival-analysis-clinical`  
Parent Claude Science skill: `clinical-trial-analysis`

## Purpose

Perform right-censored clinical survival analysis using Kaplan-Meier estimation, Cox proportional hazards regression, proportional-hazards testing, and risk stratification.

## When to use

- Estimate overall or stratified Kaplan-Meier survival curves with confidence intervals and risk tables.
- Estimate prognostic associations with multivariable Cox proportional hazards regression.
- Stratify patients by the Cox linear predictor and test the proportional-hazards assumption.

## Inputs

- Clinical data with a numeric time-to-event column and a binary event indicator where zero is censored and one is an event. (required)
- At least 50 patients and at least 20 events are recommended for reliable Cox estimates. (optional)
- Optional stratification variable and Cox covariates. (optional)
- Optional precomputed risk scores from an upstream analysis. (optional)

## Outputs

- Cox coefficients with hazard ratios, confidence intervals, p-values, and recorded reference levels.
- Patient-level risk scores, risk-group assignments, annotated clinical data, and survival summaries.
- Proportional-hazards diagnostics, missingness assessment, key metrics, and a sensitivity analysis when the global proportional-hazards test fails.
- Kaplan-Meier, forest, risk-group, Schoenfeld-diagnostic, and cumulative-hazard figures.

## Workflow

1. Identify the cohort and enumerate every time-to-event endpoint carried by the data.
2. For each endpoint, load the corresponding time and event columns and run Kaplan-Meier and Cox analyses.
3. Test proportional hazards with Schoenfeld residuals and run time-split and stratified sensitivity analyses when the global test is significant.
4. Calculate the Cox linear predictor, define risk groups with the selected split method, and estimate optimism-corrected discrimination.
5. Export canonical metrics, reference levels, missingness results, model artifacts, and plots for each endpoint before reporting.

## Decision rules

- Analyze every endpoint present in a cohort unless a skipped endpoint and its reason are explicitly documented.
- Use the optimism-corrected C-index as the headline discrimination metric because the apparent C-index is biased upward.
- When events per variable are below ten, describe discrimination as potentially overfitted or unreliable.
- When the proportional-hazards assumption is violated, label hazard ratios as time-averaged and potentially misleading in result tables and figure captions.
- When a median is not reliably reached, report it as not reached and use landmark survival rates.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_4ea8b6c8a3d1565b` — User-provided clinical right-censored survival dataset: The user supplies time, event, endpoint, stratification, and covariate definitions.

### Secondary resources

- `rr_086a92cdd06bd8cb` — Rotterdam Breast Cancer example cohort: A no-download example with overall-survival and recurrence-free-survival endpoints is requested.
- `rr_7d497735aded74fa` — NCCTG Lung Cancer example cohort: A small built-in overall-survival demonstration is requested.

### Fallback resources

- `rr_b24fab3feb17e789` — Rotterdam or Lung built-in cohort: The TCGA clinical package is unavailable or its download fails.

### Optional resources

- `rr_96bbf1dbd79055f7` — R survival package: Kaplan-Meier estimation, Cox proportional hazards regression, and built-in example cohorts.
- `rr_79c39bfe833fae1b` — survminer: Enhanced Kaplan-Meier curves and risk tables.
- `rr_ea25075ebb47fa0c` — ggplot2, ggprism, and scales: Survival-analysis visualization.
- `rr_4c164f0890ed5149` — Kaplan-Meier estimator: Nonparametric survival-curve estimation under right censoring.
- `rr_682a344d385230d1` — Cox proportional hazards regression: Multivariable hazard-ratio estimation and linear-predictor risk stratification.

## Validation / QC

- Copy all reported sample counts, hazard ratios, intervals, p-values, discrimination metrics, and reference levels from exported result tables.
- Match the reported model specification to the covariates present in the exported coefficient table.
- Report informative missingness and any follow-up anomaly prominently.
- Evidence requirement: Use the endpoint-specific exported metrics and diagnostic tables as the canonical source for every quantitative claim.
- Evidence requirement: State the event definition, reference groups, number of events, complete-case exclusions, proportional-hazards result, and events per variable.

## Failure handling

- All candidate covariates are constant or exceed the missingness threshold.
- The full Cox model is collinear or fails to converge.
- The event indicator is not encoded as a binary zero-or-one variable.
- An example cohort dependency or download is unavailable.
- Fallback rule: When the full Cox model fails, use the workflow's stepwise fallback and inspect individual covariate results.
- Fallback rule: When the TCGA example cannot be loaded, use the Rotterdam or Lung example cohort.
- Fallback rule: When a median is not reached or its confidence limit is missing, use landmark survival estimates instead.

## Limitations

- The workflow supports right censoring but not competing risks, interval censoring, multi-state models, or restricted mean survival time.
- The Cox analysis is complete-case and does not perform multiple imputation.
- Risk scores and risk groups are fitted and evaluated in the same cohort; bootstrap correction does not replace external validation.
- A proportional-hazards sensitivity analysis is diagnostic rather than a replacement for the primary model.

## Important domain-specific rules

- Enumerate and analyze every endpoint carried by a cohort rather than silently selecting one.
- Export canonical quantitative tables before narrative reporting and source every reported statistic from those artifacts.
- Pair Cox estimates with proportional-hazards diagnostics, missingness review, events-per-variable assessment, and optimism correction.

## Portability boundary

- Mandatory invocation of bundled skill-local R loader, workflow, plotting, and export scripts with platform success tokens. — migration action: `exclude_or_capability_map`
- Biomni LiteratureSearch and pdf-report-generation calls. — migration action: `exclude_or_capability_map`
- Biomni-specific results directory layout, report packaging, GenerateImage use, and Phylo branding. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
