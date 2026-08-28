---
name: biomarker-and-signature-analysis
description: "Analyze sparse biomarker panels, cross-cohort disease signatures, longitudinal progression trajectories, or treatment-response signatures with explicit cohort, leakage, effect-size, and replication controls. Use when a user asks for biomarker selection, consensus omics signatures, pseudotime/progression features, or response-signature enrichment; keep the modes and estimands distinct."
---

# Biomarker and signature analysis

## Purpose

Route four related but non-interchangeable designs: sparse binary biomarker
selection, cross-cohort disease-signature synthesis, longitudinal progression,
and treatment-response signature analysis. Preserve the estimand and replication
unit instead of treating every signature task as differential expression.

## When to use

| Mode | Distinguishing contract |
|---|---|
| `sparse-panel` | Expression/abundance matrix plus binary outcome; nested validation and stability selection |
| `consensus-disease-signature` | At least two independent cohorts with a common two-group contrast and effect/SE inputs |
| `longitudinal-progression` | Repeated patient/timepoint measurements; at least 10 patients with at least 3 timepoints each |
| `response-enrichment` | Longitudinal treatment cohorts, gene sets, response definition, and independent-cohort concordance policy |

## Inputs

- `sparse-panel`: genes-by-samples matrix, aligned binary outcome, optional
  pre-filtered features, and an independent validation cohort if available.
- `consensus-disease-signature`: cohort identifiers, platform/data type,
  group mapping, tissue/timepoint filters, control type, FDR and core-effect
  thresholds.
- `longitudinal-progression`: positive-valued feature matrix, patient and
  numeric timepoint metadata, batch and clinical covariates, and sampling
  regularity.
- `response-enrichment`: drug, disease, gene sets, tissue/context, response
  thresholds or categorical labels, minimum mapped-set size, and cohort-role
  policy.

## Workflow

1. Classify the mode and state its outcome, contrast, time axis, or response
   estimand. Block on a cross-sectional input for a longitudinal question.
2. Validate sample identifiers, outcome/contrast labels, units, cohort
   independence, feature identifiers, missingness, and access scope.
3. For `sparse-panel`, perform filtering and scaling inside resampling folds;
   fit elastic-net/LASSO with nested cross-validation and stability selection.
   Keep discovery CV, selection-biased locked-panel CV, and independent
   validation as separate estimates.
4. For `consensus-disease-signature`, remove likely duplicate deposits, fit
   per-cohort effects and standard errors, pool with random-effects synthesis,
   apply FDR and sign-consistency rules, define core genes by an effect floor,
   and run control-type sensitivity analysis when needed.
5. For `longitudinal-progression`, retain a positive scale, choose TimeAx for
   irregular sampling, linear mixed models for regular sampling, or a hidden
   Markov model for discrete stages. Test linear/quadratic/cubic dynamics with
   FDR and validate pseudotime against within-patient and clinical measures.
6. For `response-enrichment`, save all discovered cohorts before curation,
   preserve near-miss exclusions and roles, map gene sets with coverage gates,
   score with the declared GSVA version, compare baseline-to-endpoint changes,
   and retain independence-aware concordance and threshold sensitivity.
7. Reconcile every headline value with its source table; export the design,
   parameters, exclusions, estimates, uncertainty, and provenance.

## Resource selection

- Prefer user-provided matrices and metadata when the mode contract is met.
- For public discovery or validation, the registry records GEO, ArrayExpress /
  BioStudies, GO, Reactome, MSigDB Hallmark, and related adapters. Treat these
  names as replaceable resources; inspect the registry record for access and
  license status before use.
- Use the full meta-tested feature universe for enrichment background. Keep
  Hallmark or small collections when memory constraints are documented.
- For response mode, GSVA `1.50.5` is a hard requirement from the archived
  contract; do not silently substitute another version. Categorical response
  labels are an explicit lower-scope fallback when severity cannot be
  recovered.

## Decision rules

- Do not call a discovery-only panel validated. Report independent validation
  as unavailable when it was not performed.
- Pool effects and standard errors, not p-value votes or concatenated cohorts.
  Keep heterogeneity, control type, and contributing-cohort count visible.
- Do not Z-score before TimeAx; use within-patient monotonicity as the primary
  trajectory quality measure and disclose its threshold interpretation.
- Use unpaired rank-sum testing for responder versus non-responder endpoint
  deltas; reserve paired testing for matched pharmacodynamic pre/post data.
- Report focused and global FDR separately and make independent-only concordance
  the headline when parent-study splits could cause pseudo-replication.

## Evidence

Keep cohort records, effect/standard-error calculations, model predictions,
cross-cohort inferences, and recommendations as distinct claim classes. Preserve
accessions, versions, exclusions, and replication status with the claim.

## Validation

- Input gate: design/mode, sample and patient alignment, cohort independence,
  feature mapping, scale, contrast, and required replication.
- Intermediate gate: leakage checks, duplicate/cohort checks, effect/SE and
  heterogeneity checks, coverage and exclusion ledger, FDR denominator, and
  trajectory monotonicity or response sensitivity.
- Output gate: preserve discovery catalogs, validation status, uncertainty,
  raw and adjusted statistics, exact mapped features, and the reason for every
  skipped or unavailable analysis.

## Failure handling

Stop on incompatible designs, missing outcome/time labels, leakage, unresolved
feature mappings, or insufficient repeated measurements. Use only declared
fallbacks: reduce folds or adjust the penalization route for convergence,
DerSimonian–Laird when random-effects REML cannot fit, seed-feature testing
when no trajectory feature passes FDR, and categorical response labels when
numeric severity is unavailable. Label every fallback and do not merge its
estimand with the primary result.

## Outputs

Provide mode-specific tables and figures, a manifest of cohorts/features and
exclusions, performance or effect estimates with uncertainty, validation and
sensitivity status, and an interpretation that distinguishes observation,
model prediction, inference, and recommendation.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling](../_shared/failure_handling.md).

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`consensus-disease-signature`](references/source_workflows/consensus-disease-signature/WORKFLOW.md) — Derive a direction-consistent consensus transcriptional signature across two or more independent human bulk-transcriptome cohorts using random-effects effect-size meta-analysis.
- [`disease-progression-longitudinal`](references/source_workflows/disease-progression-longitudinal/WORKFLOW.md) — Reconstruct disease-progression trajectories from longitudinal patient omics, identify trajectory-associated features, stratify progression rates, and validate computational staging.
- [`elastic-net-biomarker-panel`](references/source_workflows/elastic-net-biomarker-panel/WORKFLOW.md) — Select a minimal, interpretable binary-outcome biomarker panel from high-dimensional omics data using elastic-net or LASSO logistic regression, nested cross-validation, stability selection, and optional independent validation.
- [`signature-response-enrichment`](references/source_workflows/signature-response-enrichment/WORKFLOW.md) — Test whether residual on-treatment gene-signature activity marks patients who fail a drug and whether the direction reproduces across independently discovered cohorts.

<!-- END MANAGED: SOURCE WORKFLOWS -->
