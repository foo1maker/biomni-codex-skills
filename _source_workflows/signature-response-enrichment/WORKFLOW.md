# Signature Response Enrichment

Source workflow: `signature-response-enrichment`  
Parent Claude Science skill: `biomarker-and-signature-analysis`

## Purpose

Test whether residual on-treatment gene-signature activity marks patients who fail a drug and whether the direction reproduces across independently discovered cohorts.

## When to use

- Discover longitudinal drug-response transcriptomic cohorts, score gene signatures with GSVA, compare change from baseline between responders and non-responders, and assess cross-cohort concordance.

## Inputs

- Drug name. (required)
- One or more gene signatures supplied as symbol lists, a GMT file, or MSigDB set identifiers. (required)
- Disease. (required)
- Optional response thresholds, response metric, tissue, context and focused panels, pharmacodynamic cohort policy, minimum set size, Fisher shared-parent policy, and cohort overrides. (optional)

## Outputs

- Media-checked report, standalone figures, result tables, and the complete discovery catalog with include or exclude reasons.
- Rerunnable bundle containing GSVA matrices, response tables for both thresholds, per-gene differential-expression tables, dual-FDR statistics, and both Fisher values.

## Workflow

1. Confirm inputs and the required statistical environment before analysis.
2. Search GEO and ArrayExpress or BioStudies broadly and save every candidate before filtering.
3. Apply drug-arm, longitudinal-sampling, tissue, and recoverable-response inclusion rules; retain explicit exclusion reasons and assign cohort roles.
4. Define response from baseline-to-endpoint severity improvement or use categorical response labels when numeric severity is unavailable.
5. Map each signature to the cohort feature space, report coverage, and score samples with GSVA.
6. Compare endpoint change-from-baseline scores between non-responders and responders, assess concordance, run supported endpoint and gene-level tests, analyze pharmacodynamic cohorts, and repeat threshold sensitivity.
7. Generate method-labelled figures, tables, and a report that presents adjusted and nominal evidence with independence and small-sample caveats.
8. Reconcile every headline number against source tables, validate numbering and rendering, confirm external facts, and enforce consistent statistical labels.

## Decision rules

- Hard-fail unless GSVA version 1.50.5 is available; do not silently continue with another version.
- Use an unpaired Wilcoxon rank-sum test for non-responder versus responder endpoint delta scores; paired testing is reserved for matched pharmacodynamic pre/post measurements.
- When cohorts share a parent study, report independent-only and all-splits Fisher values and default the headline to the independence-aware value in non-interactive runs.
- Report focused-family and global-family Benjamini-Hochberg FDR separately and distinguish nominal from FDR-adjusted significance.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_1cc316d93314d138` — NCBI GEO and ArrayExpress or BioStudies standard APIs: Use together for broad discovery because pivotal cohorts may occur outside GEO.

### Secondary resources

- `rr_ae11a9540281ea67` — MSigDB Hallmark and Reactome immune gene sets: Use for context scoring and focused disease-relevant module reporting.

### Fallback resources

- `rr_93704e1f0c495388` — Categorical responder and non-responder labels: Use when a numeric severity metric cannot be recovered, and flag the cohort as label-based.

### Optional resources

- `rr_ff0eb71971de1eaa` — NCBI GEO: Public longitudinal transcriptomic cohort discovery.
- `rr_6966cc025f7a62b4` — ArrayExpress / BioStudies: Public cohort discovery outside GEO.
- `rr_6bbd0513b1ac16db` — GSVA 1.50.5: Gene-set scoring with a version fixed by the archived workflow.
- `rr_7a50dd7d85c224fe` — limma: CAMERA and supported endpoint or per-gene differential analysis.
- `rr_de49180956a9de1d` — edgeR: RNA-seq count transformation for limma-voom analysis.

## Validation / QC

- Report per-cohort signature coverage and drop sets with fewer mapped genes than the configured minimum.
- Reconcile every headline value, GSVA version, and both Fisher values against the source tables before delivery.
- Confirm every accession, PMID, and DOI against the primary source or run record.
- Evidence requirement: The discovery catalog must retain all screened candidates, explicit inclusion or exclusion reasons, near-miss wrong-drug cohorts, and actual cohort roles.
- Evidence requirement: Present per-cohort effects, one-sided p-values, independent-only and all-splits Fisher values, dual FDR, and sensitivity results rather than relying on one borderline combined p-value.

## Failure handling

- A signature maps to fewer genes than the configured minimum.
- CAMERA or limma-voom is unsupported for a cohort.
- Small samples, stricter response thresholds, boundary patients, and shared-parent cohort splits make results fragile or pseudo-replicated.
- Fallback rule: Use categorical response labels when no numeric severity metric is available and flag the cohort as label-based.
- Fallback rule: With one cohort, report per-cohort tests and mark Fisher concordance as not applicable.
- Fallback rule: Auto-skip unsupported CAMERA or limma-voom analyses with an explicit note while retaining the delta-GSVA analysis.

## Limitations

- The workflow is limited to longitudinal bulk transcriptomics and excludes single-cell and spatial data in version 1.
- Dataset selection requires agent curation and the packaged pipeline had not been executed end-to-end on real data as an integrated workflow at capture time.

## Important domain-specific rules

- Auditable cohort discovery and curation with explicit near-miss exclusions and role assignment.
- Response-table construction, coverage-gated GSVA, delta-score testing, independence-aware concordance, dual FDR, pharmacodynamic controls, and threshold sensitivity.

## Portability boundary

- TodoWrite, AskUserQuestion, ManageMachine, internal script paths, and platform output-tree orchestration. — migration action: `exclude_or_capability_map`
- Internal report-builder, media-output-check loop, and fixed ReportLab glyph and layout directives. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
