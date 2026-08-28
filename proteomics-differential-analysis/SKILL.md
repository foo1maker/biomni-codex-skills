---
name: proteomics-differential-analysis
description: Analyze TMT or LFQ protein-intensity data with limma and PSM-count-aware DEqMS variance correction, including QC, reproducible exports, and explicit replicate and threshold requirements.
---

# Purpose

Perform differential protein-expression analysis for TMT or LFQ mass-spectrometry
data while preserving intensity, PSM-count, metadata, model, diagnostic, and
threshold provenance. This skill is not for RNA-seq, metabolomics, or precomputed
fold changes without underlying intensities.

# When to use

Use when protein intensities, biological replicates, condition metadata, and a
contrast are available. PSM/peptide counts are strongly preferred because DEqMS
uses them for variance correction. Require at least two biological replicates per
condition and recommend three or more.

# Inputs

- Required: protein-by-sample intensity matrix, PSM-level table with gene/
  protein identifiers or a protein-level intensity table, and metadata with a
  condition column.
- Strongly recommended: PSM/peptide counts per protein.
- Optional: requested contrast, adjusted-p threshold, fold-change threshold,
  imputation method, normalization method, and supported file format.
- Record whether intensities are raw/log2, missingness, protein identifiers,
  replicate unit, comparison, and all input hashes.

# Workflow

1. Validate matrix orientation, identifiers, metadata/condition mapping,
   replicate counts, missingness, and PSM information. Fail before modeling if
   the input is actually RNA-seq, metabolomics, or a precomputed fold-change
   table.
2. Use the reproducible limma-plus-DEqMS route. Default to MinProb imputation
   and median normalization; use kNN, quantile, or no normalization only when
   requested and record the choice.
3. Fit the specified condition contrast, generate intensity/missingness/PCA/
   correlation/QC and differential-expression plots, and inspect the variance-
   versus-PSM relationship.
4. Export full and filtered DEqMS tables, normalized matrix, PSM counts, model
   object, and Markdown summary. Build a PDF only from these validated artifacts
   when explicitly requested.
5. Interpret with the selected thresholds. The archived default is adjusted
   p <0.05 and |log2FC| >0.58; preserve relaxed or stringent user choices.

# Resource selection

Consult `../../03_resource_registry/resource_registry.yaml`. `limma` and DEqMS
are the primary analysis adapters; ExperimentHub/example data are secondary QA
inputs, not evidence for user data. ggplot2, ComplexHeatmap, and related plotting
packages are optional adapters. Use the packaged workflow/documented equivalent
when available; do not assume packages, example downloads, or PDF tooling are
installed. Record versions, access, and license status.

# Decision rules

- Keep PSM-aware p-values and counts in the result; do not silently substitute a
  standard model when DEqMS inputs are available.
- Use the default adjusted-p/fold-change thresholds only unless the user
  specifies otherwise. No significant proteins is a result to diagnose, not a
  reason to relax thresholds automatically.
- Inspect PCA/contrast and missingness before changing filters. Report all
  normalization/imputation choices.
- Use base-R SVG fallback when an optional SVG device is unavailable; disclose
  the fallback. Use a report fallback only when dedicated PDF assembly is
  unavailable and disclose it.

# Validation

- Check replicate counts, condition mapping, intensity distributions before/
  after normalization, missingness, PCA, sample correlations, and DEqMS
  variance-versus-PSM diagnostics.
- Verify full and filtered tables contain logFC, PSM-aware p-values, adjusted
  p-values, counts, and denominators. Retain normalized matrix, PSM counts,
  metadata, fitted model, and full results for reproducibility.
- Require the documented stage completion checks or equivalent artifact checks;
  validate plots, report text, and artifact schemas before handoff.
- Mark any imputation, filtering, threshold, missing package, or fallback and
  keep the analysis scope limited to protein quantification.

# Failure handling

On failure, retry missing dependencies or transient example-data access, then
modify/adapt only with a documented reason; do not jump to unverified inline
analysis. If all proteins are filtered, inspect missingness and adjust the
declared filter. If no proteins are significant, inspect PCA and contrast before
considering relaxed thresholds. If SVG/PDF support is unavailable, preserve
validated PNG/Markdown/CSV outputs and disclose the fallback.

# Outputs

Return full and significant result tables, normalized matrix, PSM counts,
top-protein table, reusable model object, QC/DE plots, and a Markdown summary.
Return PDF only when requested and assembled from validated artifacts. Include
contrast, replicates, preprocessing, thresholds, PSM-aware statistics,
limitations, citations, provenance, and validation status.

# Shared policies

- [Evidence policy](../_shared/evidence_policy.md)
- [Resource selection policy](../_shared/resource_selection.md)
- [Provenance policy](../_shared/provenance_policy.md)
- [Validation policy](../_shared/validation_policy.md)
- [Failure handling policy](../_shared/failure_handling.md)

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`proteomics-diff-exp`](references/source_workflows/proteomics-diff-exp/WORKFLOW.md) — Perform differential protein-expression analysis for TMT or LFQ mass-spectrometry data with limma linear models and DEqMS PSM-count-aware variance correction.

<!-- END MANAGED: SOURCE WORKFLOWS -->
