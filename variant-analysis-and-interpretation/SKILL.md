---
name: variant-analysis-and-interpretation
description: Route genomic variant work across raw sequencing variant calling, functional annotation, and clinical allelic-series or actionability interpretation. Use when a user needs an auditable variant workflow with build, evidence tier, uncertainty, and source provenance kept explicit; do not infer clinical significance from calling alone.
---

# Variant analysis and interpretation

## Purpose

Maintain a staged variant evidence chain from reads to candidate variants,
annotations, and clinical interpretation. Keep raw calling, functional
annotation, and actionability as distinct modes and make genome build,
evidence version, phenotype, and uncertainty visible.

## When to use

Use for germline or somatic variant calling from FASTQ/BAM/CRAM, annotation of
an existing VCF, or position-resolved clinical evidence and actionability
review. Route structural-only, expression-only, or protein-design questions to
their modality-specific workflows.

## Inputs

- Declare one mode: `variant_calling`, `functional_annotation`, or
  `clinical_interpretation`.
- Calling mode: sample sheet, FASTQ/BAM/CRAM, reference genome, known-sites or
  truth set when available, caller/configuration, coverage/QC thresholds, and
  germline/somatic design.
- Annotation mode: VCF, genome build, transcript policy, consequence fields,
  population/frequency sources, and prediction/annotation versions.
- Clinical mode: normalized variant identifiers, gene/transcript, phenotype,
  family context, evidence records, clinical guidelines, and actionability
  rubric.

## Workflow

1. Freeze sample identity, reference/build, transcript policy, caller or
   annotation versions, and input hashes. Check sample swaps, contamination,
   coverage, and coordinate conventions.
2. In `variant_calling`, run read/alignment QC, call variants, apply declared
   quality/coverage filters, compare replicates or truth data when available,
   and export a VCF plus a call-level QC ledger.
3. In `functional_annotation`, normalize alleles and IDs, annotate consequence,
   population frequency, predicted effect, and gene/transcript context. Keep
   missing or conflicting annotations instead of silently dropping records.
4. In `clinical_interpretation`, build a variant/allelic-series evidence table,
   reconcile ClinVar-like and disease/oncology evidence, assign the configured
   evidence/actionability tier, and record phenotype and source limitations.
5. Export each stage separately and link downstream claims to the exact variant,
   build, evidence record, and retrieval/version manifest.

## Resource selection

Select optional callers, annotation tools, genome references, population
databases, and clinical evidence catalogs through the resource registry. Common
adapters include Ensembl/VEP, ClinVar, CIViC, gnomAD, dbSNP, and gene/variant
registries, but none is assumed available. Prefer pinned reference builds and
stable record identifiers. If a source is restricted, stale, unreachable, or
license-incompatible, mark that evidence axis unavailable and preserve the
query; do not substitute an unversioned database.

## Decision rules

- Keep `variant_calling` upstream of annotation and `clinical_interpretation`;
  a high-quality call is not pathogenicity or actionability.
- Require exact build, allele representation, transcript, and normalization
  before joining evidence. Treat unresolved liftover or allele mismatches as
  explicit conflicts.
- Separate population frequency, functional prediction, clinical assertions,
  phenotype fit, and expert interpretation. A single source or prediction is
  not sufficient for a high-confidence clinical claim.
- Preserve somatic versus germline design, family/segregation context, and
  tumor purity or clonality assumptions where applicable.
- Use a documented evidence rubric and label findings as observation,
  prediction, inference, or recommendation; do not provide patient-specific
  medical advice.

## Validation

Check input/sample identity, reference/build and transcript policy, coverage and
call QC, allele normalization, VCF validity, duplicate and multiallelic
handling, annotation release, population frequency, evidence identifiers,
phenotype match, conflicts, and actionability-tier justification. Verify that
all reported variants can be traced to a call or source record and that no
filtered record disappeared without a reason.

## Failure handling

- If sample identity, build, reference, or allele representation is unresolved,
  stop the affected stage and return a mapping/conflict report.
- If coverage or call quality is insufficient, report the limitation and do not
  promote the variant to clinical interpretation.
- If annotation is incomplete, preserve the variant with missing fields and
  label the affected evidence axis unassessed.
- If clinical sources disagree or are unavailable, retain each assertion,
  downgrade confidence, and return an evidence-gap report rather than forcing
  a consensus.
- If a caller or reference is unavailable, return a reproducible configuration
  and validated partial result; never invent a variant or consequence.

## Outputs

Return stage-labelled VCF/tables, QC and filtering ledgers, annotation and
evidence matrices, conflicts and missingness, actionability tiers with rubric
and citations, build/version/input manifests, and a conservative interpretation
that separates call, annotation, clinical evidence, and recommendation.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling](../_shared/failure_handling.md) to every stage.

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`clinical-variant-allelic-series`](references/source_workflows/clinical-variant-allelic-series/WORKFLOW.md) — Build a position-resolved catalogue for one human gene by reconciling clinically observed ClinVar alleles with curated CIViC evidence, assigning actionability and mechanism annotations, adding UniProt domain context, and producing auditable tables, adaptive figures, and a report.
- [`genetic-variant-annotation`](references/source_workflows/genetic-variant-annotation/WORKFLOW.md) — Annotate variants in a VCF with functional consequences and available clinical or population evidence, then produce filtered tables, gene summaries, and auditable reports.
- [`variant-calling-from-sequencing`](references/source_workflows/variant-calling-from-sequencing/WORKFLOW.md) — Produce a quality-controlled germline SNV and indel call set from sequencing reads, alignments, or existing VCFs, with dual-caller concordance and optional truth-set accuracy benchmarking.

<!-- END MANAGED: SOURCE WORKFLOWS -->
