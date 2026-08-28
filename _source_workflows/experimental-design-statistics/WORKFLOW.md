# Experimental Design Statistics

Source workflow: `experimental-design-statistics`  
Parent Claude Science skill: `experimental-design-and-plate-layout`

## Purpose

Plan two-group count-based omics experiments using analytic power and sample-size calculations, batch-balanced layouts, multiple-testing guidance, and sensitivity analysis.

## When to use

- Estimate power for a proposed two-group bulk RNA-seq or analogous count-based experiment.
- Recommend sample size under per-gene and FDR-aware power assumptions.
- Construct and validate a batch-balanced laboratory layout.
- Compare multiple-testing strategies and practical design constraints.

## Inputs

- Assay type, two-group independent design, planned sample count, and sample relationship. (required)
- Expected fold change and biological variability from pilot data or a literature-based coefficient of variation. (required)
- Target power, alpha, and multiple-testing preference. (required)
- Budget, sample availability, batch structure, sequencing depth, and covariates. (optional)

## Outputs

- Power calculations, sample-size recommendation, and sensitivity tables for coefficient of variation, DE proportion, and mean count.
- Power-versus-sample-size and tissue-CV reference figures.
- Batch assignment template, confounding validation, and batch-design figure.
- Statistical analysis plan, laboratory checklist, and machine-readable design parameters.
- Reusable batch-design and design-parameter objects.

## Workflow

1. Use pilot data to estimate biological variability when available; otherwise select an explicit literature-based tissue coefficient of variation.
2. Calculate per-gene power for the proposed mean count, sample size, coefficient of variation, fold change, and alpha.
3. Estimate the FDR-aware sample size and use it for the experimental plan.
4. Construct batches that cross condition and balance the documented covariates.
5. Generate a complete recommendation including sensitivity sweeps for CV, DE proportion, and mean count.
6. Plot the power curve across a range that includes the recommended sample size and validate batch confounding and covariate balance.
7. Derive exported design parameters from the computed recommendation and pass the consistency gate before saving artifacts.

## Decision rules

- Use this implementation only for independent two-group, one-to-one allocation under the analytic negative-binomial framework.
- Prefer measured pilot variability over literature estimates.
- Use the FDR-aware sample size rather than the smaller per-gene estimate for the experimental plan.
- Batch must cross condition; no downstream method can recover a biological effect when batch and condition are confounded.
- Always run sensitivity analyses for coefficient of variation and assumed proportion of differentially expressed genes.
- Plan at least three to four samples per group even if an analytic calculation suggests two.
- Use BH-FDR as standard, IHW when more power is desired, and Bonferroni when stringent family-wise control is required.
- Prioritize additional biological samples over deeper sequencing for differential-expression power once adequate depth is reached.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_41db1dbed20b2bbc` — Pilot count data or a DESeq2 object: Study-specific biological variability can be measured.

### Secondary resources

- `rr_b8908bb96c429bb2` — Bundled tissue-specific coefficient-of-variation database: Pilot data are unavailable.

### Fallback resources

- `rr_13779f4285e894b7` — Default literature-based coefficient of variation 0.4 with sensitivity analysis: The sample type is uncertain and no pilot data are available.

### Optional resources

- `rr_8a7a79093107ed07` — DESeq2: Pilot count-data handling and variability estimation.
- `rr_ce20aa17dbcdc1c3` — RNASeqPower: Per-gene analytic power calculation.
- `rr_8d51abca53017dd2` — RnaSeqSampleSize: FDR-aware sample-size calculation.
- `rr_33932e0b88956593` — pwr: Continuous-readout power calculations including qPCR.
- `rr_e8623e790a6be56e` — IHW: Independent-hypothesis-weighting option for multiple testing.
- `rr_2d8dd61de8f43652` — anticlust: Covariate-balanced batch assignment.
- `rr_f3a4936deaf955aa` — ggplot2: Power and batch-design figures.
- `rr_4ab821cdd9f04d0b` — Two-group unpaired negative-binomial power analysis and sample-size estimation for bulk RNA-seq: Calculates two-group per-gene power from expected count, biological variability, fold change, sample size, and alpha.
- `rr_a32cb8d2e1e21824` — RnaSeqSampleSize: Estimates sample size while controlling false discovery across many genes.
- `rr_25d10152a60e41dc` — Anticlustering: Optimizes distribution of known covariates across laboratory batches.

## Validation / QC

- Estimate biological rather than technical coefficient of variation.
- Verify that the power-curve range reaches and passes the recommended sample size.
- Check condition-by-batch confounding and balance of key covariates.
- Require exported parameters to agree with the computed recommendation before any artifact is written.
- Use the median rather than mean coefficient of variation after filtering low-count genes.
- Evidence requirement: Report the assumed or measured coefficient of variation and its tissue source.
- Evidence requirement: Keep per-gene and experiment-wide FDR-aware power estimates distinct.
- Evidence requirement: Report sensitivity to DE proportion, coefficient of variation, and expected per-gene count.
- Evidence requirement: Preserve the exact batch layout and design parameters as reusable analysis objects.

## Failure handling

- Batch is confounded with condition.
- A literature CV is treated as measured study-specific variability.
- Library size is substituted for expected per-gene count in the variance model.
- Per-gene power is presented as experiment-wide discovery power.
- A hand-written export disagrees with the computed recommendation.
- Fallback rule: Use a literature-based tissue CV with explicit sensitivity analysis when pilot data are unavailable.
- Fallback rule: If the required sample size exceeds availability, reconsider effect-size assumptions, paired or blocked designs, or budget constraints.
- Fallback rule: If condition and a covariate are confounded, redesign or model the covariate; batch balancing cannot repair the study design.
- Fallback rule: Use per-gene power only as a partial result when the FDR-aware package is unavailable, and disclose that it underestimates required sample size.

## Limitations

- Multi-factor, interaction, repeated-measures, paired, time-course, survival, unequal-allocation, and simulation-based designs are outside scope.
- The power model does not charge degrees of freedom for covariate adjustment.
- RnaSeqSampleSize assumes one dispersion and one expected count across genes.
- Anticlustering balances known covariates but is not randomization and cannot fix condition-covariate confounding.
- Several bundled tissue-CV values are literature consensus estimates rather than direct measurements.

## Important domain-specific rules

- Pilot-versus-literature variability decision gate.
- Separate per-gene and FDR-aware sample-size calculations.
- Sensitivity sweeps across coefficient of variation, DE proportion, and per-gene count.
- Condition-crossed, covariate-balanced batch assignment with confounding validation.
- Computed-recommendation-to-export consistency gate.

## Portability boundary

- Mandatory packaged R-script workflow and exact Biomni verification strings. — migration action: `exclude_or_capability_map`
- Biomni GenerateImage and pdf-report-generation report orchestration. — migration action: `exclude_or_capability_map`
- Biomni LiteratureSearch and bundled-resource awareness instructions. — migration action: `exclude_or_capability_map`
- Internal script identifiers, design-results paths, and platform report completion requirement. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
