---
name: real-world-evidence-analysis
description: Configuration-driven retrospective analysis of structured EHR or claims-style data, covering cohort definitions, comparators, baseline tables, treatment patterns, survival estimands, and guarded multivariable models. Use when a user needs an auditable observational cohort analysis from structured clinical records, not a causal treatment recommendation.
---

# Real-world evidence analysis

## Purpose

Run a configuration-driven retrospective cohort analysis while keeping cohort
construction, descriptive summaries, time-to-event estimands, and adjusted
models visibly separate. Treat the result as observational evidence: do not
present it as a causal estimate or clinical advice.

## When to use

Use for structured EHR, claims-style, encounter, diagnosis, medication,
laboratory, or outcome tables where the question specifies a cohort, exposure,
comparator, and follow-up. Route to another workflow when the data are
unstructured notes only, a prospective trial-design question, or a molecular
assay rather than a patient-level cohort.

## Inputs

- Patient, encounter, diagnosis, drug/exposure, and outcome tables in a
  documented tabular format; record table versions and row counts.
- A column map, cohort inclusion/exclusion rules, index-date definition,
  comparator mode, exposure map, outcome definition, censoring rule, and
  analysis configuration.
- Optional ICU-stay or other setting-specific tables, plus a phenotype
  validation plan and any user-supplied code lists.

## Workflow

1. Inspect schemas, identifiers, date ranges, duplicates, missingness, and
   linkage coverage. Freeze the mapping and retain excluded-row counts.
2. Define the index cohort and time origin. For disease-versus-population
   questions use a rest-of-population or setting comparator; for
   treatment-versus-treatment questions define exposure-based active arms.
3. Produce a cohort-flow ledger, denominators, baseline Table 1, and treatment
   pattern summaries before fitting any model.
4. Estimate Kaplan-Meier and pre-specified landmark outcomes with numbers at
   risk and confidence intervals. Check time origin, censoring, and
   proportional-hazards assumptions.
5. Fit an adjusted Cox model only when the configured events-per-variable
   gate is met. Run sensitivity analyses for comparator, phenotype, missing
   data, and landmark choices; write a run manifest before packaging results.

## Resource selection

Use the project resource registry as an adapter catalog, not as an implicit
dependency. Prefer the user's structured data. A public demo clinical dataset
may be used only when its access terms and requested setting match; a small
synthetic fixture is the fallback for schema and pipeline checks. Record the
dataset identifier, access state, query or download time, version, and license
status. Do not retrieve restricted records without the user's authorization.

## Decision rules

- Keep disease-versus-population, active-comparator, and single-arm descriptive
  cohorts as distinct analysis modes.
- Use the configured EPV threshold (10 is a documented default) as a hard gate
  for multivariable Cox modeling; suppress the model when events are too few.
- Label p-values as exploratory when multiplicity correction is not applied.
- Treat code-defined phenotypes, treatment exposure, and outcome definitions
  as measurement choices requiring validation, not ground truth.
- Do not infer causality, benefit, harm, or dosing from an observational
  association without a separately justified causal design.

## Validation

Check table and linkage counts, cohort-flow arithmetic, mutually exclusive
arms, index-date ordering, follow-up and censoring ranges, mortality and event
denominators, baseline missingness, landmark confidence intervals, and
proportional-hazards diagnostics. Verify that every reported estimate points to
an input definition and a stored artifact; hash or version the input tables and
configuration where possible.

## Failure handling

- If a required table, column map, exposure map, or outcome definition is
  missing, stop and list the exact missing contract rather than guessing.
- If comparator membership conflicts with exposure dates, return the conflict
  and preserve the affected rows for review.
- If EPV is insufficient, report unadjusted or pre-specified descriptive
  estimates and explicitly suppress Cox output.
- If median survival is not reached, use a pre-specified landmark summary and
  state that the median is not estimable.
- If a dataset is restricted or unreachable, use an authorized local fixture or
  synthetic validation data and label the analysis as not run on the target
  dataset.

## Outputs

Return a cohort-flow table, baseline Table 1, treatment/exposure summaries,
survival estimates and plots, model diagnostics, and (only if gated) Cox
results. Include a configuration and provenance manifest, excluded-row counts,
limitations, and a concise interpretation that distinguishes observation,
calculation, inference, and recommendation.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling](../_shared/failure_handling.md) to every run.

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`real-world-evidence`](references/source_workflows/real-world-evidence/WORKFLOW.md) — Run a configuration-driven retrospective cohort study on structured EHR or claims-style data, including cohort construction, baseline characterization, treatment-pattern summaries, survival analysis, and guarded multivariable modeling.

<!-- END MANAGED: SOURCE WORKFLOWS -->
