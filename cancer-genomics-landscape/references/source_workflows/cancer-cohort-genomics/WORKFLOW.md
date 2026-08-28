# Cancer Cohort Genomics

Source workflow: `cancer-cohort-genomics`  
Parent Claude Science skill: `cancer-genomics-landscape`

## Purpose

Quantify non-silent somatic mutation and high-level copy-number alteration frequencies for one or more genes across cBioPortal cohorts by cancer type, compare cohorts, characterize hotspot or allele patterns, and report the results.

## When to use

- Measure mutation, amplification, deep-deletion, and combined alteration frequencies by cancer type.
- Compare matched cancer types across cohorts.
- Summarize recurrent mutation hotspots or dispersed allele patterns.

## Inputs

- One or more HUGO gene symbols. (required)
- Cohort selection or study keyword; if omitted, TCGA PanCancer Atlas plus a large MSK-IMPACT cohort is auto-selected. (optional)
- Optional cancer-type filter, report toggle, and literature-grounding toggle. (optional)

## Outputs

- Per-cancer-type frequency table covering all selected cohorts.
- Hotspot and allele tables when the gene has recurrent hotspots.
- Selected figures in SVG and PNG formats.
- A report containing an infographic, methods, results, conclusions, references, caveats, and next steps.

## Workflow

1. Clarify genes, cohorts, figure selection, report depth, and whether literature grounding is enabled.
2. Resolve gene identifiers and selected cohort molecular profiles.
3. Derive sequenced and copy-number-profiled sample sets and use their intersection as the common denominator.
4. Fetch non-silent mutation records and high-level amplification and deep-deletion calls.
5. Split mixed cohorts by cancer type, compute frequency rows, concatenate results, and validate denominator invariants.
6. Generate hotspot outputs only for genes with recurrent hotspots; otherwise report dispersed alleles.
7. Create only applicable figures and perform visual checks.
8. Ground biological context in retrieved literature when enabled, then build and validate the report.

## Decision rules

- Use the intersection of sequenced and copy-number-profiled samples as the common denominator so combined alteration frequency cannot be below either component frequency.
- Skip an assay for a study when the corresponding molecular profile is unavailable, report the available assay, and mark the unavailable value as not available.
- Generate hotspot bins and a hotspot figure only when recurrent hotspots are detected.
- Do not attribute pooled cohort frequency differences to biology without accounting for case mix.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_fb52f88fc88d3558` — cBioPortal public REST API: Retrieving aggregate somatic mutation, copy-number, and non-identifying cancer-type attributes.

### Secondary resources

- `rr_4e898a4245532e0b` — Literature search: Grounding gene biology, top cancer types, therapeutic context, and report references.

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- `rr_8f43998cd31a3498` — cBioPortal: Public source of aggregate somatic mutation, high-level copy-number, and cancer-type data.
- `rr_f446edf915b11e32` — TCGA open-access tier: Default public cancer cohort data retrieved through cBioPortal.

## Validation / QC

- Require at least one cohort with a valid mutation or copy-number profile and successful gene resolution.
- Confirm combined alteration frequency is at least the mutation and amplification frequencies for every row.
- Require every frequency to be between 0 and 100 with a nonzero denominator, and verify CSV row counts against cancer-type counts.
- Validate selected figures visually and require a multi-page, nontrivial, text-extractable report.
- Evidence requirement: Include the required TCGA/GDC acknowledgment and both cBioPortal citations in every report.
- Evidence requirement: Use only retrieved literature records for citations; do not fabricate references.

## Failure handling

- A cohort may lack one molecular assay, requiring partial mutation-only or copy-number-only reporting.
- Purity, stromal dilution, and case-mix differences can shift observed frequencies.
- Fallback rule: When an assay profile is missing, retain the study for its available assay and mark the unavailable measurement as not available.
- Fallback rule: When recurrent hotspots are absent, omit the hotspot figure and report the leading alleles and dispersed pattern.

## Limitations

- The workflow does not analyze structural variants or fusions, survival associations, mutual exclusivity, or raw-read variant calling.
- Cross-cohort cancer-type matching is best-effort, pooled rates reflect case mix, and only GISTIC +2 amplification and -2 deep deletion are counted.

## Important domain-specific rules

- Common-denominator computation across molecular assays with an explicit invariant check.
- Adaptive hotspot-versus-dispersed-allele reporting.
- Explicit data-source attribution, denominator disclosure, and report acceptance checks.

## Portability boundary

- Biomni LiteratureSearch and GenerateImage tool IDs. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation skill, Phylo branding, Read media checks, package script names, and /mnt/results runtime paths. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
