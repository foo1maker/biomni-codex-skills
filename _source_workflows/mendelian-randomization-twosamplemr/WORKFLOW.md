# Mendelian Randomization Twosamplemr

Source workflow: `mendelian-randomization-twosamplemr`  
Parent Claude Science skill: `genetic-causal-and-risk-analysis`

## Purpose

Assess causal direction between an exposure and outcome using genetic instruments and two-sample GWAS summary statistics.

## When to use

- Estimate the causal effect of an exposure on an outcome from two GWAS summary-statistic datasets.
- Assess instrument heterogeneity, directional pleiotropy, reverse causation, and weak-instrument risk.

## Inputs

- Exposure and outcome OpenGWAS trait identifiers. (optional)
- Exposure and outcome GWAS summary-statistic files with SNP, beta, standard error, p-value, effect allele, and other allele columns. (optional)
- Effect-allele frequency. (optional)
- Instrument-selection p-value threshold and LD-clumping threshold. (optional)

## Outputs

- MR estimates from IVW, MR-Egger, weighted-median, and weighted-mode methods with effect, uncertainty, significance, instrument count, and F statistics.
- Heterogeneity, pleiotropy, directionality, harmonized-data, single-SNP, and leave-one-out result tables.
- MR scatter, forest, funnel, and leave-one-out plots.
- A serialized analysis object and report.

## Workflow

1. Load exposure and outcome data from OpenGWAS identifiers or user files and harmonize alleles.
2. Select and clump genetic instruments, then run the primary MR estimators.
3. Run heterogeneity, pleiotropy, directionality, single-SNP, and leave-one-out sensitivity analyses.
4. Generate diagnostic plots and export all estimates, harmonized data, sensitivity results, and the analysis object.

## Decision rules

- Use a genome-wide instrument threshold of 5×10^-8 and LD-clumping r² of 0.001 unless the analysis specifies alternatives.
- Treat agreement in direction and significance across IVW, MR-Egger, weighted median, and weighted mode as stronger evidence; discuss every discordant or non-significant method.
- Treat an MR-Egger intercept p-value below 0.05 as a directional-pleiotropy warning.
- When Cochran Q p-value is below 0.05, flag heterogeneity and run MR-PRESSO if available.
- Treat incorrect Steiger direction as a reverse-causation concern and F statistics below 10 as weak-instrument risk.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_e9e27a42df5631dc` — OpenGWAS trait records: Valid exposure and outcome trait identifiers are available.

### Secondary resources

- `rr_fa9e43217f2f60b3` — User-provided GWAS summary-statistic files: The exposure and outcome are supplied as compatible local tables.

### Fallback resources

- `rr_0ea9f73e6cbd3f7a` — Unclumped harmonized instruments: The LD-clumping API is unavailable; retain a warning that LD may affect the results.

### Optional resources

- `rr_2453d6d3c2d8d203` — OpenGWAS: Source of exposure and outcome GWAS summary statistics addressed by trait identifiers.
- `rr_909888587c53120d` — TwoSampleMR: Two-sample Mendelian-randomization estimators and harmonization.
- `rr_022ef6c3d7d4da0e` — ieugwasr: OpenGWAS access.
- `rr_f008913f6707ec1e` — MR-PRESSO: Optional outlier detection when heterogeneity is significant.
- `rr_5382b9347a486138` — Cochran Q test: Instrument heterogeneity assessment.
- `rr_4a48290330bbf452` — MR-Egger intercept: Directional-pleiotropy assessment.
- `rr_127210328dc92408` — Steiger directionality test: Causal-direction assessment.
- `rr_55b72c2c082156c6` — Leave-one-out analysis: Effect stability to removal of individual instruments.
- `rr_d043794be21d300b` — Inverse-variance weighted MR: Primary multi-instrument causal estimator.
- `rr_e058f7c868942429` — MR-Egger: Pleiotropy-robust causal estimator with intercept test.
- `rr_6d459ab14d6a356f` — Weighted-median MR: Robust causal estimator included in method concordance.
- `rr_c4644967f7db1fcb` — Weighted-mode MR: Robust causal estimator included in method concordance.

## Validation / QC

- Harmonize exposure and outcome alleles before estimation.
- Report heterogeneity, pleiotropy, directionality, F statistics, single-SNP, and leave-one-out sensitivity results.
- Check that exposure and outcome use compatible genome builds when harmonization removes most instruments.
- For binary outcomes, compute liability-scale Steiger R² using prevalence rather than treating the outcome as quantitative.
- Evidence requirement: Present primary causal estimates together with method concordance and all sensitivity analyses.
- Evidence requirement: Explicitly discuss non-significant or directionally discordant methods rather than dismissing them.
- Evidence requirement: Retain the SNP-level harmonized dataset and per-SNP and leave-one-out estimates.

## Failure handling

- No variants satisfy the instrument threshold.
- LD clumping fails because the OpenGWAS service is unavailable.
- Allele harmonization removes most variants because genome builds or alleles do not align.
- Steiger testing lacks required sample-size metadata or uses inappropriate binary-outcome R².
- OpenGWAS requests are rate limited.
- Fallback rule: If no instrument is found, verify the trait identifier and consider a less stringent threshold with explicit disclosure.
- Fallback rule: If clumping fails, continue without clumping only with a warning that LD may bias results.
- Fallback rule: If Steiger cannot run because sample sizes are unavailable, report that limitation and retain the other sensitivity analyses.
- Fallback rule: If dedicated report generation is unavailable, disclose use of a packaged HTML, markdown, or base-R fallback.

## Limitations

- The workflow does not support one-sample MR, nonlinear MR, or multivariable MR with more than two exposures.
- Weak instruments, heterogeneity, pleiotropy, and reverse causation weaken causal interpretation.
- Unclumped instruments may remain correlated when the clumping service is unavailable.
- Binary-outcome directionality requires liability-scale R² inputs.

## Important domain-specific rules

- Harmonize exposure and outcome alleles before any causal estimation.
- Triangulate causal claims across multiple estimators, heterogeneity, pleiotropy, directionality, and leave-one-out evidence.
- Use explicit weak-instrument and reverse-causation gates before interpreting significance causally.
- Preserve harmonized SNP-level evidence and method-specific estimates for auditability.

## Portability boundary

- Mandatory skill-local R scripts and exact success-message checks. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation routing and packaged-report fallback. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
