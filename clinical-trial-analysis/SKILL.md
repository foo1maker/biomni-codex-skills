---
name: clinical-trial-analysis
description: "Route clinical-trial design simulation, registry landscape description, or right-censored survival analysis with explicit estimands, configuration/query provenance, validation gates, event counts, and uncertainty. Use for operating characteristics, registered-trial landscapes, Kaplan-Meier/Cox analyses, or risk stratification; do not claim protocol approval, treatment efficacy, or clinical advice."
---

# Clinical trial analysis

## Purpose

Provide one portable router for three distinct modes: prospective design
simulation, descriptive registry landscape mapping, and analysis of completed or
observational right-censored outcomes. Keep their questions, data, and claims
separate.

## When to use

| Mode | Minimum contract |
|---|---|
| `design-simulation` | One configuration for endpoint/design/adaptation/effects plus error-control and analytic-benchmark gates |
| `registry-landscape` | Conditions, status/phase filters, mechanism taxonomy, and a documented registry query |
| `survival-analysis` | Numeric time-to-event, binary event indicator, covariates/strata, and endpoint definition |

## Inputs

- Design: endpoint, hypotheses, treatment/control assumptions, prevalence,
  accrual/dropout, adaptation, effect scenarios, sensitivity grid, simulation
  preset, and reporting assumptions.
- Landscape: disease terms/configuration, default active statuses, mechanism,
  sponsor, recruitment, phase, geography, and design filters.
- Survival: every time/event endpoint, event coding, cohort/strata, covariates,
  missingness, follow-up anomalies, and optional precomputed risk scores.

## Workflow

1. Classify the mode and state the estimand, population, endpoint, and evidence
   boundary. Do not treat registry description as trial efficacy analysis.
2. Validate configuration/query/endpoint, identifiers, units, censoring, access
   scope, and denominator metadata.
3. In `design-simulation`, use one configuration as the source of truth; run
   global and least-favorable-null FWER/type-I gates plus a reduced analytic
   benchmark before reporting operating characteristics. Include a null
   scenario, sensitivity sweep, and Monte Carlo standard error. Use quick runs
   for iteration and a thorough run before quoting final proportions.
4. In `registry-landscape`, query the documented ClinicalTrials.gov API v2
   scope, retain query parameters and all returned records, classify mechanisms
   conservatively, and aggregate phase/sponsor/geography/design. Broaden filters
   only under a recorded no-result rule.
5. In `survival-analysis`, enumerate every endpoint, fit Kaplan-Meier and Cox
   models, record reference levels and complete-case exclusions, test
   proportional hazards, and run time-split/stratified sensitivity when needed.
   Compute risk groups and optimism-corrected discrimination only with clear
   in-sample/external-validation labels.
6. Reconcile tables, configuration/query parameters, model diagnostics, and
   figures before narrative reporting.

## Resource selection

Prefer user data/configuration or the authoritative registry/API scope. The
registry catalogs ClinicalTrials.gov/API v2, `rpact`, R survival/survminer, and
related adapters; inspect
`../../03_resource_registry/resource_registry.yaml` for access/license/version.
Named adapters are replaceable and not guaranteed available. Built-in example
cohorts are fallback demonstrations, not external validation.

## Decision rules

- Stop design reporting if either error-control or analytic-benchmark gate
  fails. Treat simulated type-I control as verified under tested nulls, not a
  mathematical proof.
- Report registry counts only for returned records under documented conditions,
  statuses, and filters. Vague intervention descriptions remain unclassified;
  sponsor subsidiaries are normalized with a recorded rule.
- Analyze every survival endpoint unless a skip and reason are recorded. Use
  optimism-corrected C-index as the headline discrimination metric, flag fewer
  than ten events per variable, and label non-proportional hazard ratios as
  time-averaged/potentially misleading.
- If a survival median is not reached, report `not_reached` and landmark rates.
  Right-censoring support does not imply competing-risks or multi-state support.

## Evidence

Separate registry observations, simulated operating characteristics, fitted
survival estimates, inferences, and recommendations. Retain query/configuration
identifiers, event counts, uncertainty, model diagnostics, and access scope.

## Validation

- Input gate: mode/estimand, configuration/query, endpoint/event/censoring,
  sample/event counts, access status, and license/registry scope.
- Intermediate gate: design gate outputs and MC error; registry query coverage,
  exclusions, taxonomy status; survival missingness, PH diagnostics, events per
  variable, and model specification consistency.
- Output gate: numerical claims match canonical tables, all filters/assumptions
  are retained, uncertainty and limitations are visible, and no fallback or
  example is presented as primary evidence.

## Failure handling

Stop on failed simulation gates, unreachable registry, unresolved endpoint or
event coding, collinear/non-convergent models, or insufficient event support.
Use declared recovery only: bounded retry and documented filter broadening for
registry queries, quick-to-thorough rerun for design simulation, or a declared
stepwise/example-cohort survival fallback. Preserve the original scope and mark
partial/blocked status.

## Outputs

Return mode-specific configurations/query manifests, operating-characteristic
or trial tables, survival estimates/diagnostics, uncertainty and sensitivity,
figures, evidence classes, and limitations. Explicitly state that outputs do
not approve a protocol, establish efficacy/safety, or provide clinical advice.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling](../_shared/failure_handling.md).

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`clinical-trial-design-simulation`](references/source_workflows/clinical-trial-design-simulation/WORKFLOW.md) — Simulate and evaluate a two-arm confirmatory design with one primary endpoint, estimate operating characteristics across configured scenarios, require type-I/FWER and analytic-benchmark validation gates, and produce auditable tables, figures, and a report.
- [`clinicaltrials-landscape`](references/source_workflows/clinicaltrials-landscape/WORKFLOW.md) — Map the registered clinical-trial landscape for a disease by mechanism, phase, sponsor, geography, and study design using ClinicalTrials.gov data.
- [`survival-analysis-clinical`](references/source_workflows/survival-analysis-clinical/WORKFLOW.md) — Perform right-censored clinical survival analysis using Kaplan-Meier estimation, Cox proportional hazards regression, proportional-hazards testing, and risk stratification.

<!-- END MANAGED: SOURCE WORKFLOWS -->
