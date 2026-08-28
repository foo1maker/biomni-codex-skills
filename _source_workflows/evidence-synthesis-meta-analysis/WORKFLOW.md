# Evidence Synthesis Meta Analysis

Source workflow: `evidence-synthesis-meta-analysis`  
Parent Claude Science skill: `literature-and-evidence-synthesis`

## Purpose

Pool compatible effect estimates across studies with a random-effects meta-analysis, robustness diagnostics, risk-of-bias assessment, and verified citations.

## When to use

- Run a meta-analysis from a user-supplied extraction table.
- Discover, screen, and extract studies for a literature-driven meta-analysis.
- Evaluate heterogeneity, influence, leave-one-out stability, subgroups, and small-study diagnostics.

## Inputs

- Extraction table with one comparison per row, containing study, effect measure, effect, confidence interval or standard error, sample sizes, subgroup, design, and source identifier. (optional)
- Research question for literature-driven study discovery and extraction. (optional)
- One compatible effect measure for the analysis. (required)

## Outputs

- Validated extraction table, screening log, risk-of-bias table, and references used.
- Pooled meta-analysis estimates, reusable fitted model, leave-one-out results, influence diagnostics, and small-study test.
- Forest, funnel, leave-one-out, and heterogeneity figures in PNG and SVG.
- A final evidence-synthesis report.

## Workflow

1. Confirm that the requested design and effect measure are in scope and that only one compatible measure will be pooled.
2. Validate a supplied extraction table or discover and PRISMA-screen studies from the research question.
3. Extract one compatible between-group effect and uncertainty estimate per independent comparison.
4. Verify every effect value and bibliographic field against its source before analysis.
5. Fit the documented random-effects model with heterogeneity, prediction interval, and optional subgroup testing.
6. Interpret leave-one-out, influence, and small-study diagnostics without automatically deleting influential studies.
7. Create a structured narrative risk-of-bias assessment for each study.
8. Inspect every result figure and assemble the verified synthesis, limitations, and references.

## Decision rules

- Never mix mean differences with odds ratios or other incompatible measures in one pool.
- Log-transform OR, RR, and HR before pooling and back-transform the summary estimate.
- Prefer an intention-to-treat or treatment-policy estimand and one independent arm per study.
- Use random-effects REML with Hartung-Knapp confidence interval and a prediction interval as the default.
- Investigate influential studies rather than deleting them automatically.
- Interpret Egger or funnel asymmetry as publication bias only when at least 10 studies are available.
- Stop rather than approximate single-arm proportion, network, multivariate, individual-participant, or diagnostic-test-accuracy meta-analysis.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_3504098b8baec4c0` — User-supplied extraction table matching the documented template: Structured study effects and uncertainties have already been extracted.

### Secondary resources

- `rr_13fc8c7bbfad76ce` — Published primary studies identified from a research question: Literature-driven discovery, screening, and extraction are required.

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- `rr_7b3e16573ac536c7` — meta: Meta-analysis engine and diagnostics.
- `rr_5a49cf84729a19e3` — metafor: Random-effects pooling and influence diagnostics.
- `rr_f3a4936deaf955aa` — ggplot2: Diagnostic figures.
- `rr_03b6f0ca22cc5563` — ggrepel: Readable diagnostic labels.
- `rr_8db715ce7dbe586f` — patchwork: Composite diagnostic figures.
- `rr_cf163fde985c291c` — generic-inverse-variance random-effects pooling: Pools compatible study effects while allowing between-study heterogeneity.
- `rr_d7cdafebdedeb9c0` — Hartung-Knapp CI: Default uncertainty adjustment for the pooled effect.
- `rr_be85ab383c6f4070` — META_TAU=DL: Documented alternative when explicitly selected.

## Validation / QC

- Validate that every row has an effect plus a confidence interval or standard error, a consistent measure, and a source identifier.
- Check for duplicate cohorts and dependent extensions before pooling.
- Verify each numerical effect and complete citation against the original record.
- Confirm that no single study reverses the conclusion in leave-one-out analysis.
- Evidence requirement: Record every inclusion and exclusion decision with a reason in a screening log.
- Evidence requirement: Retain the between-group effect, uncertainty, estimand, design, and source identifier for each pooled comparison.
- Evidence requirement: Do not reconstruct missing numbers or citations from memory.
- Evidence requirement: Describe risk of bias as a structured narrative aid rather than an automated RoB2 score.

## Failure handling

- Incompatible measures or estimands are pooled together.
- Duplicate cohorts or dependent study extensions are double-counted.
- A fixed-effect model is used despite genuine heterogeneity.
- Funnel asymmetry is labeled publication bias with fewer than 10 studies.
- Effect values or citations are fabricated or recovered from memory.
- Fallback rule: Use the documented DerSimonian-Laird tau estimator only when selected and disclose the deviation.
- Fallback rule: If exact transcript wording is unavailable, verify against the fetched source and state that exact-wording recovery was unavailable.
- Fallback rule: If an effect cannot be extracted and verified, exclude the study with a documented reason.

## Limitations

- Single-arm prevalence, network or multivariate, individual-participant, and diagnostic-test-accuracy meta-analyses are outside version-one scope.
- One compatible effect measure is required per analysis.
- Small-study diagnostics are unreliable when fewer than 10 studies are available.
- Risk-of-bias output is a structured narrative aid, not an automated formal score.

## Important domain-specific rules

- Dual entry mode for supplied extraction tables or literature-driven discovery.
- PRISMA-style inclusion and exclusion log with duplicate-cohort protection.
- Blocking numerical and citation verification before synthesis.
- Random-effects pooling with prediction interval and subgroup testing.
- Leave-one-out, influence, and small-study robustness suite.

## Portability boundary

- Biomni LiteratureSearch discovery and references.jsonl execution-trace storage. — migration action: `exclude_or_capability_map`
- Biomni Read media-output-check and transcript-path verification. — migration action: `exclude_or_capability_map`
- Biomni GenerateImage conceptual infographic. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation, packaged report scripts, internal /mnt/results paths, and notebook machine identifiers. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
