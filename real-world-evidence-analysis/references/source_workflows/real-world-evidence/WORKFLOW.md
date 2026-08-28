# Real World Evidence

Source workflow: `real-world-evidence`  
Parent Claude Science skill: `real-world-evidence-analysis`

## Purpose

Run a configuration-driven retrospective cohort study on structured EHR or claims-style data, including cohort construction, baseline characterization, treatment-pattern summaries, survival analysis, and guarded multivariable modeling.

## When to use

- Define diagnosis-code cohorts and contemporaneous population or active comparators, build baseline Table 1, summarize treatment patterns, and analyze time-to-event outcomes.

## Inputs

- Patient, encounter, diagnosis, and drug tables in CSV, TSV, or Parquet format. (required)
- ICU-stay table. (optional)
- Column mapping, cohort definitions, comparator mode, exposure maps, outcome definitions, and analysis configuration. (required)

## Outputs

- Cohort-flow, baseline, treatment, survival, comparison, and optionally Cox-model result tables.
- Survival and summary figures plus an analysis report.

## Workflow

1. Select the demo or user data, define the cohort and comparator configuration, and map the required table columns.
2. Construct the index cohort and comparator, document cohort flow and denominators, and generate baseline characteristics and treatment-pattern summaries.
3. Estimate Kaplan-Meier and landmark survival, evaluate proportional-hazards assumptions, and fit a Cox model only when the events-per-variable gate is satisfied.
4. Ground disease, treatment, and outcome context in literature and report methods, results, caveats, and generated artifacts.

## Decision rules

- Use a rest-of-population or rest-of-ICU comparator for disease-versus-population questions and an exposure-defined active comparator for treatment-versus-treatment questions.
- Define active-comparator arms from drug exposure maps rather than from diagnosis-code vintage.
- Fit the multivariable Cox model only when events per variable meet the configured minimum, stated as 10 by default.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_2b658894d5d4df44` — User-supplied structured clinical tables: Use for the user's retrospective cohort question.

### Secondary resources

- `rr_5c4ee0f638479bae` — MIMIC-IV demo: Use for a reproducible 100-patient ICU worked example.

### Fallback resources

- `rr_409aa9b6a69cda4c` — Small synthetic fixture matching the canonical schema: Use for pipeline validation when PhysioNet is unreachable.

### Optional resources

- `rr_ab511db13dc87720` — MIMIC-IV: Restricted-access full clinical dataset requiring PhysioNet credentials and agreements.
- `rr_95d0e2e781dcf6e9` — data.table: Structured table processing.
- `rr_174e67d08b009156` — survival: Kaplan-Meier, landmark, and Cox analyses.
- `rr_f3a4936deaf955aa` — ggplot2: Clinical cohort and survival visualization.
- `rr_5fd734c7de5899d4` — Multivariable Cox proportional-hazards model: Adjusted time-to-event analysis only when the events-per-variable gate is met.

## Validation / QC

- Validate cohort flow, denominators, mortality denominator, and generated artifact existence.
- Report landmark survival with 95 percent confidence intervals and numbers at risk, and document Cox gating and proportional-hazards checks.
- Evidence requirement: Ground disease, treatment, and outcome context in retrieved literature and validate code-defined phenotypes against chart review or validated phenotype definitions.
- Evidence requirement: Label p-values exploratory and disclose that multiple-testing correction is not applied.

## Failure handling

- Comparator configuration and exposure maps are inconsistent or the drugs table is missing.
- Cox modeling is suppressed because events per variable are insufficient.
- Median survival is not reached or an incorrect time origin produces misleading comparator mortality.
- Fallback rule: Suppress the Cox model when the events-per-variable requirement is not met.
- Fallback rule: Report landmark survival when median survival is not reached.

## Limitations

- The study is descriptive rather than causal and is vulnerable to confounding by indication.
- Code-defined phenotypes require validation, small cohorts produce wide late-time confidence intervals, and full MIMIC-IV access requires credentialing and agreements.

## Important domain-specific rules

- Configuration-driven cohort and comparator construction, canonical input tables, baseline Table 1, treatment maps, Kaplan-Meier and landmark summaries, and EPV-gated Cox analysis.

## Portability boundary

- Biomni LiteratureSearch, GenerateImage and media-output-check orchestration, internal script and workspace paths, Phylo branding, and downstream skill names. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
