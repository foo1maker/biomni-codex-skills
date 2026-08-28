---
name: multi-omics-integration
description: Integrate two or more aligned omics views with latent-factor modeling, preserve incomplete sample overlap, quantify shared and view-specific variance, and export interpretable factor scores and feature weights.
---

# Purpose

Fit an unsupervised multi-view latent-factor model and make factor
interpretation traceable to view-level variance, feature weights, sample
scores, and optional clinical associations. The result is an integrative
analysis, not a supervised prediction or a claim of shared biology from scores
alone.

# When to use

Use when at least two feature-by-sample omics matrices describe overlapping or
partially overlapping samples and the user wants shared/view-specific
variation, factor scores, or cross-view feature rankings. Do not use for a
single matrix or when the requested task is a framework-specific single-cell
workflow.

# Inputs

- Required: named list of at least two feature-by-sample matrices and at least
  ten samples per view.
- Optional: sample metadata with IDs and clinical variables; view-type labels
  including binary views; requested factor count.
- Record sample-ID overlap, feature filtering, likelihood choice per view,
  assay units, missingness, and all input artifact identifiers/hashes.

# Workflow

1. Confirm view dimensions, sample identifiers, factor count, binary views, and
   clinical metadata. Reject a one-view request.
2. Align sample identifiers without discarding incomplete overlap merely to
   force a complete-case matrix. Preserve per-view missingness explicitly.
3. Start with 15 factors unless the user chooses another value; use a smaller
   factor count for a small or constrained dataset. Use a Bernoulli likelihood
   for binary views.
4. Fit the MOFA+ model and record software/model versions, convergence, and
   randomization settings when available.
5. Export the model, sample-by-factor scores, per-view feature weights,
   variance explained per factor/view and per view, and top features.
6. Interpret factors jointly from cross-view variance, within-view variance,
   high-weight features, sample scores, and available clinical associations.

# Resource selection

Consult `../../03_resource_registry/resource_registry.yaml` and record an
explicit resource role, version, access status, and license status. MOFA2/MOFA+
is the primary modeling adapter when available; supporting plotting packages
such as ComplexHeatmap, ggprism, circlize, reshape2, RColorBrewer, and
rmarkdown are optional adapters. Do not assume any package or example data is
installed. Use an equivalent documented latent-factor implementation only as a
declared secondary route with its changed assumptions recorded.

# Decision rules

- Require at least two views and ten samples per view.
- Permit incomplete sample overlap because the model can handle missing view
  entries; report the overlap rather than silently dropping samples.
- Reduce the factor count to roughly 5–10 when convergence or sample size is
  limiting. If memory is limiting, filter each view to its 5,000 most variable
  features and disclose the filter.
- Treat high variance in one view as view-specific and high explained variance
  across views as shared signal. Low total explained variance means the view is
  poorly explained; it is not evidence of absence of biology.
- Use exported scores and weights as the only handoff to downstream
  stratification, survival, biomarker, or pathway analyses.

# Validation

- PASS the input gate only after dimensions, IDs, view types, likelihoods,
  sample overlap, and sample unit are recorded.
- Check convergence, per-view and per-factor variance explained, retained
  samples/features, and exported score/weight tables after fitting.
- Use clinical-association plots only when metadata match the score rows.
- Check that every interpretation can be traced to exported variance, weights,
  scores, or a cited clinical variable. Keep PNG output if SVG support fails.
- Label outputs as unsupervised model predictions or inferences, not measured
  cross-omics effects.

# Failure handling

Block a one-view input, unresolved sample IDs, or fewer than ten samples per
view. On non-convergence, retry with a declared lower factor count and preserve
the failed attempt. On memory pressure, apply the declared variable-feature
filter and start a new labeled run. If clinical metadata or SVG support is
unavailable, omit that optional output and disclose the omission. Never force
complete overlap or silently merge incompatible views.

# Outputs

Return the fitted model artifact, sample-factor score table, per-view weights,
variance-explained tables, top features, diagnostic/interpretive figures, and
an analysis summary. Include view dimensions, overlap, factor count,
likelihoods, convergence, filtering, limitations, and provenance for every
model-derived claim. A report is an optional presentation adapter.

# Shared policies

- [Evidence policy](../_shared/evidence_policy.md)
- [Resource selection policy](../_shared/resource_selection.md)
- [Provenance policy](../_shared/provenance_policy.md)
- [Validation policy](../_shared/validation_policy.md)
- [Failure handling policy](../_shared/failure_handling.md)

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`multi-omics-integration`](references/source_workflows/multi-omics-integration/WORKFLOW.md) — Integrate two or more omics views with MOFA+ to learn interpretable latent factors that capture shared and view-specific variation, tolerate incomplete sample overlap, quantify variance decomposition, and provide factor scores and feature weights for downstream analysis.

<!-- END MANAGED: SOURCE WORKFLOWS -->
