# Disease Progression Longitudinal

Source workflow: `disease-progression-longitudinal`  
Parent Claude Science skill: `biomarker-and-signature-analysis`

## Purpose

Reconstruct disease-progression trajectories from longitudinal patient omics, identify trajectory-associated features, stratify progression rates, and validate computational staging.

## When to use

- Order longitudinal samples along disease pseudotime despite irregular sampling.
- Identify features that change monotonically or non-monotonically along the trajectory.
- Stratify patients by progression rate and relate trajectory position to clinical outcomes.

## Inputs

- Feature-by-sample matrix of normalized positive omics or clinical measurements. (required)
- Sample metadata containing sample_id, patient_id, and numeric timepoint. (required)
- Outcome, treatment, batch, and clinical covariates. (optional)

## Outputs

- Per-sample disease pseudotime assignments.
- Trajectory-associated features with polynomial degree, fit statistics, and direction.
- Per-patient progression summaries and statistics for all tested features.
- Reusable trajectory model and model metadata with quality metrics.
- Trajectory, progression-rate, feature-trend, clinical-validation, and uncertainty figures.

## Workflow

1. Confirm longitudinal data availability, data type, study design, disease context, goals, and method preference.
2. Load the feature matrix and metadata, enforce patient and timepoint requirements, and preprocess by data type.
3. Check positive-valued scale, batch structure, and the number of retained variable features.
4. Align patient trajectories with TimeAx or use the documented alternative suited to the study design.
5. Assign pseudotime and identify linear, quadratic, and cubic trajectory features with FDR correction.
6. Estimate patient progression rates and generate trajectory and feature-dynamics figures.
7. Validate pseudotime against clinical stage or outcomes and report within-patient monotonicity.
8. Export assignments, feature statistics, patient summaries, model object, and metadata.

## Decision rules

- Require at least 10 patients with at least three timepoints each and prefer 20 or more patients.
- Use TimeAx for irregular sampling, linear mixed models for regular sampling, and hidden Markov models for discrete stages.
- Do not Z-score values before TimeAx because negative values disable its ratio mechanism.
- Use within-patient monotonicity as the primary quality metric because the TimeAx robustness function can be misleading.
- Interpret within-patient monotonicity above 0.5 as good and 0.3 to 0.5 as moderate.
- If genome-wide FDR identifies no features, test the TimeAx seed features and report the documented fallback.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_61bc3fdbba5bc561` — User-supplied longitudinal omics matrix and patient metadata: At least 10 patients and three timepoints per patient are available.

### Secondary resources

- `rr_035bfd27d064b0b6` — GSE128959 bladder-cancer recurrence dataset: A published TimeAx demonstration is requested.

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- `rr_da9c064a94083225` — TimeAx: Primary multiple-trajectory alignment method for irregular longitudinal sampling.
- `rr_78609fcc8c7914c6` — numpy: Numerical processing.
- `rr_178baeb270b10c13` — pandas: Tabular preprocessing and outputs.
- `rr_6aba97b6dceeffd7` — scikit-learn: Dimensionality reduction and supporting analysis.
- `rr_980072f6e7547905` — statsmodels: Statistical modeling.
- `rr_0404370e1c073613` — lifelines: Outcome and survival analysis support.
- `rr_32a338ae79d4e363` — lme4: Alternative linear mixed-effects modeling.
- `rr_95ec32fb39850ae1` — lmerTest: Inference for the linear mixed-effects alternative.
- `rr_7c64d21dfafb90c9` — hmmlearn: Optional hidden Markov model alternative.
- `rr_32759e44dac0ad56` — sva: ComBat batch correction for the demonstration dataset.
- `rr_c7b429cd4d820e26` — TimeAx multiple trajectory alignment: Aligns irregularly sampled patient trajectories into a common disease pseudotime.
- `rr_c93925f2c5fb39a1` — Polynomial regression: Tests linear, quadratic, and cubic feature dynamics along pseudotime with FDR correction.
- `rr_a00d65ca7896bfaf` — Linear mixed-effects model: Alternative for regularly sampled longitudinal data.
- `rr_890fe069507983b3` — Hidden Markov model: Alternative when disease progression is represented as discrete stages.

## Validation / QC

- Verify at least 10 patients and at least three timepoints per patient.
- Check that samples do not cluster primarily by batch in PCA.
- Report within-patient monotonicity and correlation with clinical measures.
- Keep TimeAx input positive and avoid Z-score normalization.
- Validate feature-level FDR and document any seed-feature fallback.
- Evidence requirement: Retain pseudotime, polynomial degree, fit statistics, direction, and FDR for each reported trajectory feature.
- Evidence requirement: Support computational staging with clinical stage, outcomes, or another available clinical measure when possible.
- Evidence requirement: Record preprocessing parameters and model quality metrics in model metadata.

## Failure handling

- Negative values from Z-scoring disable the TimeAx ratio mechanism.
- Uncorrected batch effects drive trajectory structure.
- More than 20,000 features exhaust memory during alignment.
- The TimeAx robustness function returns a misleading negative value.
- Fallback rule: Use within-patient monotonicity instead of the TimeAx robustness statistic.
- Fallback rule: Reduce to 5,000 to 10,000 variable features when memory is limiting.
- Fallback rule: Apply ComBat before trajectory analysis when samples cluster by batch.
- Fallback rule: Use seed-feature nominal testing when no genome-wide FDR-significant trajectory feature is found.
- Fallback rule: Retain the text summary when PDF assembly is unavailable.

## Limitations

- The workflow requires repeated measurements and is not a single-cell branching-trajectory method.
- TimeAx ratio mode requires positive-valued inputs.
- Ten patients is only the minimum; 20 or more are recommended for robust results.
- Alternative linear mixed or hidden Markov models are selected according to sampling and stage assumptions.

## Important domain-specific rules

- Longitudinal data eligibility and positive-scale gate.
- Irregular patient-trajectory alignment to a common pseudotime.
- FDR-controlled polynomial feature dynamics along pseudotime.
- Within-patient monotonicity and clinical-stage validation.

## Portability boundary

- Packaged load, trajectory, plotting, and export scripts with mandatory verification strings. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation orchestration and platform-managed report packaging. — migration action: `exclude_or_capability_map`
- Internal result paths and packaged model-inference script identifiers. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
