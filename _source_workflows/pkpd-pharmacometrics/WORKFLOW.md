# Pkpd Pharmacometrics

Source workflow: `pkpd-pharmacometrics`  
Parent Claude Science skill: `pkpd-modeling`

## Purpose

Run a staged population pharmacometrics workflow that validates concentration-time and optional response data, diagnoses concentration-effect delay, fits population PK and optional PD models, evaluates them, and optionally performs target-gated exposure-response dose simulation.

## When to use

- Fit nonlinear mixed-effects population PK models to a single concentration-time dataset.
- Select and fit an optional PD structure from the observed concentration-effect delay.
- Evaluate fitted PK/PD models with diagnostics, parameter precision, and visual predictive checks.
- Simulate exposure-response and derive a model-based dose only when an explicit therapeutic target window is available.

## Inputs

- Tidy long-format concentration-time table with subject ID, time, dependent variable, and at least one dose event. (required)
- Event ID, missing-DV flag, endpoint labels, and compartment labels when needed to distinguish dosing and multiple endpoints. (optional)
- Covariates such as weight, age, or sex for optional allometric scaling and covariate testing. (optional)
- User-supplied or literature-cited therapeutic target window on the modeled endpoint, required before any numeric dose is emitted. (optional)

## Outputs

- Validated tidy dataset and validation summary, including reported mappings, coercions, exclusions, and endpoint counts.
- PK parameter table and optional PD parameter table, exposure-response tables, and dose tables when target-gated simulation is run.
- Raw-data, goodness-of-fit, individual-fit, visual-predictive-check, optional PD, exposure-response, and steady-state figures in PNG and SVG.
- Saved fitted-model objects for reproducibility.
- PDF report containing the data validation summary, model choices, diagnostics, parameter results, target provenance, limitations, and next steps.

## Workflow

1. Install and verify the pharmacometric environment; do not model until nlmixr2 loads and a trivial rxode2 model compiles.
2. Map the submitted columns to the canonical schema, validate them, report every exclusion, save the tidy dataset, and set PK_ONLY when no response endpoint exists.
3. Plot concentration and optional response over time, measure the delay signature, and inspect hysteresis.
4. Fit a one- or two-compartment population PK model with FOCEi, random effects, residual error, and optional weight allometry.
5. When response data exist, fit PD sequentially with fixed individual PK estimates and choose direct, effect-compartment, or indirect-response structure from the measured delay.
6. When an explicit target window exists, simulate regimens, summarize exposure and effect, interpolate target-achieving doses, and include time to steady state.
7. Assemble and validate the final report after reconciling target provenance, model diagnostics, tables, figures, caveats, and dose disclaimers.

## Decision rules

- Fail clearly when required schema items cannot be supplied or derived; never silently discard rows.
- If no distinct response endpoint is present, set PK_ONLY and skip PD and dose stages while still producing a PK report.
- Choose one versus two PK compartments by inspection and objective-function evidence rather than hardcoding the structure.
- Choose direct Emax/Imax for negligible delay, an effect compartment for moderate hysteresis, and an indirect-response model for a large delay; surface the measured-delay justification and allow user override.
- Refuse to output any numeric dose unless the target window was supplied by the user or supported by a recorded literature citation.
- Always run goodness-of-fit plots, parameter precision, condition number, and VPC; run bootstrap intervals only when the user explicitly requests the computationally expensive upgrade.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_0b1e552f0910b491` — nlmixr2 and rxode2: Population NLME estimation and regimen simulation are required.
- `rr_a3e05bf7b7653477` — User-supplied therapeutic target window: The user provides an endpoint-specific range for target-gated dose simulation.

### Secondary resources

- `rr_5624a48bbedb95b3` — LiteratureSearch: A therapeutic target window is not supplied and a sourced window may be available.
- `rr_3c9c35990e4bb10f` — nlmixr2data public example datasets: A demonstration or QA run is requested.

### Fallback resources

- `rr_162d05904d3a8e89` — PK-only workflow: The dataset has concentrations but no distinct response endpoint.
- `rr_d527865d18f5b021` — No-dose result with a request for a target window: Neither a user-supplied nor literature-cited therapeutic target is available.

### Optional resources

- `rr_e27731e8dd65e364` — nlmixr2: Population PK/PD nonlinear mixed-effects estimation with FOCEi.
- `rr_c65d196882376e8b` — rxode2: ODE model compilation and regimen simulation.
- `rr_1467782cb6a8c25f` — ggplot2 and ggprism: Diagnostic and exposure-response figures.
- `rr_4ed6caf7221225da` — vpc or tidyvpc: Visual predictive checks.
- `rr_d1fe7be615dc4bb8` — scripts/00_setup_env.sh through scripts/06_report.py: Packaged staged environment, validation, fitting, simulation, and reporting templates.
- `rr_b01b20f1581d0378` — LiteratureSearch: Retrieve and record evidence for a therapeutic target window.
- `rr_abbf99b8d8602a79` — pdf-report-generation: Build and validate the final report.
- `rr_06ae20021c8bdbb7` — One- or two-compartment population PK model: First-order absorption, log-normal between-subject variability, and combined residual error.
- `rr_87f0b5c606169767` — Direct Emax or Imax model: PD option when effect tracks concentration with negligible delay.
- `rr_c8763daa0feddd20` — Effect-compartment model: PD option for moderate concentration-effect hysteresis.
- `rr_9d2f80d32091d721` — Indirect-response or turnover model: PD option for a large delayed response.

## Validation / QC

- Require nlmixr2 to load and a trivial rxode2 ODE to compile before fitting begins.
- Report subject, dose-event, endpoint-observation, missing/zero-DV, negative/duplicate-time, no-dose, and no-observation counts.
- Inspect OBS versus PRED/IPRED, CWRES versus time and prediction, parameter uncertainty, condition number, and a VPC.
- Validate the PDF for page count, file size, extractable text, and representative-page media defects.
- Evidence requirement: Record the user-supplied or literature-cited target window and show its provenance in the report before presenting any numeric dose.
- Evidence requirement: Attach the methodological-illustration and non-clinical-guidance disclaimer to every dose output and state observed-design extrapolation limits.
- Evidence requirement: Report parameter estimates with asymptotic percent RSE, 95% confidence intervals, between-subject variability, shrinkage, and the measured delay supporting PD-structure selection.

## Failure handling

- The required pharmacometric packages or compilers are unavailable, so models cannot be compiled or estimated.
- Required data items cannot be mapped or derived, or invalid rows would otherwise be silently lost.
- FOCEi reports poor or false convergence.
- A numeric dose is requested without an explicit sourced target window.
- Fallback rule: Degrade gracefully to PK-only when response data are absent.
- Fallback rule: When FOCEi convergence is poor, refit with foceiControl(outerOpt="bobyqa").
- Fallback rule: Run LiteratureSearch or ask the user for a target; otherwise omit numeric dosing.

## Limitations

- The workflow is not clinical dosing guidance and does not support NONMEM control streams, population meta-analysis, or PBPK.
- The workflow models one dataset with nonlinear mixed effects.
- Sequential PK-to-PD fitting fixes PK estimates and does not propagate PK uncertainty into PD or dose estimates.
- Single-dose-level designs make alternative-dose predictions extrapolations beyond observed data.
- Small or imbalanced cohorts limit covariate and pharmacogenetic inference.
- Surrogate endpoints are explicit modeling assumptions.

## Important domain-specific rules

- Explicit canonical schema mapping with loud validation and complete exclusion accounting.
- Delay-guided selection among direct, effect-compartment, and indirect-response PD structures.
- Evidence-gated dose simulation that forbids invented therapeutic targets.
- Tiered evaluation with mandatory diagnostics and an opt-in computational bootstrap upgrade.

## Portability boundary

- TodoWrite progress tracking and fixed packaged-script staging in the Biomni runtime. — migration action: `exclude_or_capability_map`
- Biomni sandbox library path and environment-specific package/version bootstrap. — migration action: `exclude_or_capability_map`
- Phylo-branded report generation and Biomni-specific media checking. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
