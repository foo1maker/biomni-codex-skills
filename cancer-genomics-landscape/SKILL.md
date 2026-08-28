---
name: cancer-genomics-landscape
description: "Summarize cohort-level cancer alteration landscapes for declared genes and cohorts, including mutation, amplification, deep-deletion, denominators, hotspot or dispersed-allele patterns, and case-mix caveats. Use for cBioPortal/TCGA-style cohort frequency questions; do not route per-variant annotation, raw-read calling, survival, or causal interpretation here."
---

# Cancer genomics landscape

## Purpose

Measure alteration frequencies across selected cancer cohorts and cancer types
with explicit assay availability and common denominators. The result is a
descriptive cohort landscape, not a per-variant clinical interpretation.

## When to use

Use for one or more HUGO gene symbols, selected cohorts or study keywords,
optional cancer-type filters, and requests for mutation, amplification,
deep-deletion, combined-alteration, hotspot, or allele summaries. Route raw
read calling, functional/clinical annotation, survival association, structural
variant/fusion analysis, and mutual exclusivity to a separately scoped method.

## Inputs

- Resolved HUGO genes and the identifier evidence used.
- Cohort/study selection, cancer-type filter, molecular-profile availability,
  and whether literature grounding is requested.
- Optional figure/report depth and a record of access scope for each cohort.

## Workflow

1. Clarify genes, cohorts, filters, assays, and output depth; resolve gene IDs
   without silently replacing an unresolved symbol.
2. Select mutation and copy-number profiles and derive sequenced and
   copy-number-profiled sample sets. Use their intersection as the combined
   denominator when both assays contribute.
3. Fetch non-silent mutation and high-level amplification/deep-deletion calls;
   preserve assay availability and per-cohort sample counts.
4. Split mixed cohorts by cancer type, compute mutation, amplification,
   deletion, and combined frequencies, and check denominator invariants.
5. Generate hotspot bins only when recurrent hotspots are observed; otherwise
   report leading alleles and a dispersed pattern without forcing a hotspot.
6. Add retrieved literature only when records and locators are available.
   Export tables and figures from the same data and validate their counts before
   summarizing.

## Resource selection

Prefer an authoritative, open cohort record that exposes the required assay,
sample set, build, and access terms. The registry catalogs cBioPortal, the TCGA
open-access tier, UCSC, and related adapters; inspect each record in
`../../03_resource_registry/resource_registry.yaml`. Adapter availability and
license status are not implied by their names. Do not treat a cached or example
cohort as current evidence without recording its release and access status.

## Decision rules

- Use the intersection of sequenced and copy-number-profiled samples for
  combined alterations; the combined frequency must not be below either
  component frequency under the same denominator.
- If an assay is absent, retain the cohort for its available assay and mark the
  missing value `not_available`; never impute it as zero.
- Keep pooled frequency differences descriptive and disclose case mix, purity,
  stromal dilution, and profile coverage before biological interpretation.
- Use only the stated amplification/deep-deletion definitions and build;
  do not broaden them silently.

## Evidence

Treat cohort records and frequencies as database observations, computed
denominators as analysis outputs, and biological interpretation as an inference.
Attach study identifiers, profile/build, query time, and case-mix caveats.

## Validation

- Input gate: gene resolution, cohort/profile availability, cancer-type mapping,
  build and assay scope, and access/license decision.
- Intermediate gate: nonzero denominators, frequencies in [0,100], intersection
  counts, assay-missingness, and mutation/copy-number row provenance.
- Output gate: CSV rows match cancer-type counts; hotspot versus dispersed mode
  is justified; figures and citations are derived from retrieved artifacts.

## Failure handling

Classify invalid IDs, missing profiles, unavailable cohorts, and missing
literature separately. Keep partial assay results with explicit unavailable
fields. If hotspot evidence is absent, omit the hotspot figure and state why.
If access fails, do not call the landscape empty; record query scope and return
`BLOCKED` or a clearly labelled partial landscape.

## Outputs

Return cohort/cancer-type frequency tables, denominators and assay status,
hotspot or dispersed-allele tables, figures, query/access manifest, citations,
and limitations. State that frequencies are observed cohort summaries and not
clinical actionability or causal effects.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling](../_shared/failure_handling.md).

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`cancer-cohort-genomics`](references/source_workflows/cancer-cohort-genomics/WORKFLOW.md) — Quantify non-silent somatic mutation and high-level copy-number alteration frequencies for one or more genes across cBioPortal cohorts by cancer type, compare cohorts, characterize hotspot or allele patterns, and report the results.

<!-- END MANAGED: SOURCE WORKFLOWS -->
