---
name: pkpd-modeling
description: Run staged population PK/PD modeling from validated concentration-time data, select delay-aware PD structures, evaluate diagnostics, and gate any exposure-response dose simulation on a sourced therapeutic target window.
---

# Purpose

Fit and evaluate a staged population pharmacometrics model for one dataset.
Keep data validation, PK, optional PD, exposure-response simulation, and dose
communication separate. This is a modeling workflow, not clinical dosing advice.

# When to use

Use when a tidy concentration-time table needs population PK and optionally
response modeling, delay diagnosis, visual predictive checks, or target-gated
exposure-response simulation. If only concentrations exist, produce a PK-only
result. Do not use this skill for PBPK, population meta-analysis, or NONMEM
control-stream work.

# Inputs

- Required: tidy long-format concentration-time data with subject ID, time,
  dependent variable, and at least one dose event.
- Optional: event ID, missing-DV flag, endpoint/compartment labels, weight/age/
  sex covariates, and response endpoint.
- Required before numeric dose output: a user-supplied or retrieved literature
  target window for the modeled endpoint, with source and retrieval record.
- Record units, mappings, exclusions, zero/missing DV handling, dose events,
  endpoint labels, and artifact identifiers/hashes.

# Workflow

1. Verify that the pharmacometric runtime and a trivial ODE compile before
   fitting. Do not silently replace a failed environment.
2. Map columns to the canonical schema, validate loudly, and report every
   exclusion. Count subjects, doses, observations, missing/zero DVs, negative
   or duplicate times, no-dose rows, and no-observation subjects.
3. Plot concentration and response over time and inspect the delay/hysteresis
   pattern. Fit a one- or two-compartment population PK model with stated
   absorption, variability, residual-error, and covariate assumptions.
4. If response exists, fit PD sequentially using fixed individual PK estimates.
   Choose direct Emax/Imax for negligible delay, an effect compartment for
   moderate hysteresis, or an indirect-response/turnover model for a large
   measured delay. Surface the reason and permit a documented user override.
5. Run goodness-of-fit plots, residual checks, parameter precision, condition
   number, and a visual predictive check. Add bootstrap intervals only when
   explicitly requested.
6. Simulate exposure-response and interpolate target-achieving regimens only
   when the target window is sourced and recorded. Include exposure, effect,
   steady-state timing, and extrapolation caveats.
7. Reconcile model diagnostics, target provenance, tables, figures, disclaimers,
   and report outputs before handoff.

# Resource selection

Consult `../../03_resource_registry/resource_registry.yaml`. `nlmixr2` and
`rxode2` are optional primary implementation adapters when available; plotting
and VPC packages are supporting adapters. A literature source may establish a
target window only after retrieval and citation verification. Public example
data are QA inputs, not evidence for the user's study. Record versions, access,
license status, and any substitution.

# Decision rules

- Fail clearly when required fields cannot be mapped or derived; never silently
  discard rows.
- If no distinct response endpoint exists, set `PK_ONLY` and skip PD/dose
  stages while retaining a PK report.
- Select one versus two PK compartments using diagnostics and objective-function
  evidence rather than hardcoding.
- Refuse every numeric dose when the target window is absent or unsupported by
  a recorded literature citation. Never invent a therapeutic range.
- Always run the mandatory diagnostic tier. Treat a poor or false convergence,
  unstable parameters, or weak predictive check as a warning/block, not a
  cosmetic issue.

# Validation

- PASS the input gate only with required schema, units, event structure,
  endpoint, subject counts, and exclusion accounting recorded.
- Verify model convergence, parameter uncertainty/percent RSE, confidence
  intervals, between-subject variability, shrinkage, condition number, residual
  diagnostics, and VPC.
- For PD selection, retain the observed delay evidence and the chosen model
  rationale. For dose simulation, retain target source, endpoint, window, and
  methodological/non-clinical disclaimer.
- Validate saved model objects, tables, figures, and report text; carry all
  caveats about sequential PK-to-PD uncertainty, extrapolation, and small or
  imbalanced cohorts.

# Failure handling

Block on missing/ambiguous schema, incompatible units, invalid dose events, or
unmapped endpoints. Preserve exclusions and partial artifacts. If response is
absent, use the declared PK-only fallback. If FOCEi convergence is poor, retry
with the documented alternative control and disclose the attempt; do not keep
changing models silently. If the target window cannot be sourced, return model
results without numeric dosing and mark the dose stage blocked.

# Outputs

Return the validated tidy dataset and summary, PK/PD parameters and diagnostics,
VPC, fitted-model artifacts, exposure-response and dose tables only when
target-gated, figures, and a report/summary with target provenance and
limitations. Clearly mark `PK_ONLY`, warnings, blocked stages, and whether any
result is observed, modeled, or extrapolated.

# Shared policies

- [Evidence policy](../_shared/evidence_policy.md)
- [Resource selection policy](../_shared/resource_selection.md)
- [Provenance policy](../_shared/provenance_policy.md)
- [Validation policy](../_shared/validation_policy.md)
- [Failure handling policy](../_shared/failure_handling.md)

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`pkpd-pharmacometrics`](references/source_workflows/pkpd-pharmacometrics/WORKFLOW.md) — Run a staged population pharmacometrics workflow that validates concentration-time and optional response data, diagnoses concentration-effect delay, fits population PK and optional PD models, evaluates them, and optionally performs target-gated exposure-response dose simulation.

<!-- END MANAGED: SOURCE WORKFLOWS -->
