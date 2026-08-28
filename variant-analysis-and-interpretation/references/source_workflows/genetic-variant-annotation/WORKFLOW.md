# Genetic Variant Annotation

Source workflow: `genetic-variant-annotation`  
Parent Claude Science skill: `variant-analysis-and-interpretation`

## Purpose

Annotate variants in a VCF with functional consequences and available clinical or population evidence, then produce filtered tables, gene summaries, and auditable reports.

## When to use

- Functional consequence annotation of germline or somatic VCF variants
- Clinical-significance and population-frequency annotation
- Impact-, rarity-, and gene-based variant prioritization

## Inputs

- A coordinate-sorted VCF conforming to VCF 4.x (required)
- Genome build and organism (required)
- Optional reference FASTA, BED intervals, or gene lists (optional)

## Outputs

- An annotated VCF
- A complete variant table and filtered high-impact, moderate-impact, and rare-variant tables
- A gene-level summary and annotation-quality figures
- Serialized results, spreadsheet export, and PDF report

## Workflow

1. Validate VCF structure, coordinate sorting, genome build, and reference-allele compatibility.
2. Choose VEP for comprehensive or clinical annotation, or SnpEff for quick annotation or non-model organisms.
3. Install or locate the selected local annotation software and compatible cache or database.
4. Annotate variants and parse consequence, clinical, and population fields into tabular form.
5. Create impact and rarity filters, gene summaries, and diagnostic plots.
6. Export machine-readable tables and package the interpretation report.

## Decision rules

- Use VEP for comprehensive or clinical annotation and SnpEff for quick workflows or non-model organisms.
- Require an explicit genome build and do not combine annotations built for a different assembly.
- Use a local VEP cache or database rather than substituting a VEP API fallback.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_f6d4c81404161d63` — Ensembl Variant Effect Predictor: Comprehensive or clinical-grade annotation fields are needed

### Secondary resources

- `rr_702620e238e75ca7` — SnpEff: A quick annotation is sufficient or the organism is not well served by the VEP path

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- `rr_5e71673aea7cf480` — ClinVar: Clinical-significance annotation
- `rr_35ab27166fd8a660` — gnomAD: Population-frequency annotation
- `rr_963335ba9f90e817` — 1000 Genomes: Population variation annotation
- `rr_462bb4a10b6e915d` — COSMIC: Somatic cancer variant annotation
- `rr_1ef5e8722da1f7b8` — Ensembl Variant Effect Predictor: Functional and clinical variant annotation
- `rr_773b576a23d0aa55` — SnpEff: Alternative functional-consequence annotation

## Validation / QC

- Validate VCF format, coordinate sorting, genome build, and REF alleles before annotation.
- Check annotation completeness and interpret missing fields in light of installed plugins and database content.
- Use the example fixture only as a workflow smoke test, not as a biological truth set.
- Evidence requirement: Preserve the annotation software, database or cache versions, genome build, and filtering thresholds with the result.
- Evidence requirement: Do not interpret automated annotations as a clinical diagnosis without expert review.

## Failure handling

- Genome-build or REF-allele mismatch can prevent correct annotation.
- Missing local caches, databases, or plugins can leave expected annotation fields absent.
- Malformed or unsorted VCF input can stop or corrupt downstream processing.

## Limitations

- Structural-variant annotation is limited.
- The workflow annotates called variants and does not perform variant calling.
- Automated annotation is not a clinical diagnosis and requires expert review for clinical use.

## Important domain-specific rules

- Assembly and REF-allele validation before annotation.
- Explicit VEP-versus-SnpEff selection based on annotation depth and organism support.
- Layered output retaining all variants alongside impact, rarity, and gene summaries.

## Portability boundary

- Automatic conda installation and fixed runtime assumptions — migration action: `exclude_or_capability_map`
- Mandatory packaged-script and relative-path execution instructions — migration action: `exclude_or_capability_map`
- Biomni Skill loading and Phylo-specific PDF reporting — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
