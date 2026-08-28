---
name: bulk-rna-and-omics-analysis
description: "Route bulk RNA and related omics workflows from raw reads, count matrices, or annotated single-cell references: FASTQ-to-counts, count-level differential expression, unsupervised clustering, and bulk composition deconvolution. Use when users need preprocessing, DE, clustering, or cell-composition estimates; keep representations, estimands, and validation gates separate."
---

# Bulk RNA and omics analysis

## Purpose

Provide a representation-aware router for raw-read quantification, count-level
differential expression, unsupervised bulk clustering, and reference-based
deconvolution. These modes share data hygiene but are not interchangeable.

## When to use

| Mode | Minimum input | Primary question |
|---|---|---|
| `fastq-to-counts` | FASTQ/accession plus matching reference and annotation | Are reads valid and can an analysis-ready gene matrix be produced? |
| `count-differential-expression` | Non-negative integer counts and aligned sample metadata | Which genes differ under a declared design/contrast? |
| `bulk-clustering` | Comparable normalized matrix | What sample/feature structure is supported by clustering diagnostics? |
| `reference-deconvolution` | Bulk expression plus annotated single-cell reference | What cell-type proportions are supported and how method-stable are they? |

## Inputs

- FASTQ or public accession, paired/single-end layout, organism, genome build,
  annotation release, and either STAR-compatible FASTA/GTF or Salmon-compatible
  transcript reference and transcript-to-gene map.
- Count matrix with sample metadata, reference level, batch/paired/interaction
  covariates, and declared count scale.
- For clustering, matrix orientation, normalization, missingness, batches,
  outliers, and optional known labels.
- For deconvolution, bulk matrix, annotated single-cell reference with cell-type
  labels and donor structure, sample metadata, gene identifier system, and
  expression scale.

## Workflow

1. Classify the representation and downstream question. Never feed TPM/FPKM to
   a raw-count model or mix STAR gene counts with Salmon transcript-derived
   estimates.
2. Validate identifiers, non-negativity/integer status where required,
   organism/build/release, sample layout, replicates, batches, and missingness.
3. In `fastq-to-counts`, check gzip/read integrity, reference sequence-name
   compatibility, raw-read QC, strandedness, and one quantification engine.
   Build the gene matrix, metadata, assignment summary, and DE-readiness handoff.
4. In `count-differential-expression`, pre-filter low counts, set the reference
   level, use a non-confounded design, fit the count model, retain unshrunk
   hypothesis-test estimates, and use shrunken effects for ranking/plots.
5. In `bulk-clustering`, choose algorithm, distance, and cluster count from
   declared geometry; compare internal metrics, stability, known labels, and
   domain plausibility rather than relying on one score.
6. In `reference-deconvolution`, harmonize gene identifiers and scale, choose a
   declared method panel, estimate proportions, compute consensus and
   concordance, and test group/longitudinal contrasts with multiplicity control.
7. Export the exact analyzed matrix, parameters, reference/build versions,
   method outputs, exclusions, and downstream handoff before interpretation.

## Resource selection

- Prefer user data and matching references. The registry catalogs ENA, GEO,
  Ensembl/GENCODE, DESeq2, edgeR, limma, BayesPrism, DWLS, MuSiC, BisqueRNA,
  CELLxGENE Census, and related software/database adapters. These are optional
  and replaceable; availability, license, and version must be checked per run.
- Use STAR or Salmon as alternatives, not as interchangeable count generators.
  Use DESeq2 for raw integer counts; route transformed inputs to a compatible
  normalized-data method.
- Treat synthetic recovery and bundled examples as smoke tests, not accuracy
  evidence. Record any excluded method because of license or access constraints.

## Decision rules

- Require exact FASTA/GTF build and sequence-name compatibility; set STAR
  `sjdbOverhang` to read length minus one when STAR is selected.
- Infer forward strandedness at forward fraction ≥0.8 and reverse strandedness
  at ≤0.2; report uncertainty when assignment support is weak.
- Keep design formula, batch balance, reference level, shrinkage method, and
  adjusted-p-value threshold explicit. Do not fit a confounded design.
- For deconvolution, an absent expected cell type in the reference is a critical
  limitation; retain per-method estimates and flag method-fragile cell types.
- For clustering, choose a geometry-compatible algorithm and document why its
  assumptions fit the matrix.

## Evidence

Label input/QC observations, fitted-model outputs, unsupervised structure,
deconvolution estimates, and downstream recommendations separately. Retain the
exact matrix, reference, method/version, and independent-validation status.

## Validation

- Input gate: format, scale, orientation, IDs, build, replicate unit, batch,
  reference coverage, and access/license decision.
- Intermediate gate: FASTQ/reference QC; DE design rank and dispersion/PCA/MA
  checks; clustering stability and metrics; deconvolution row sums, concordance,
  and independent-truth status.
- Output gate: counts or proportions, metadata, model/fit artifacts, figures,
  exclusions, and handoff schema exist and agree with logs. Keep transformed,
  synthetic, exploratory, and fallback outputs labelled.

## Failure handling

Stop on build mismatch, ID mismatch, wrong scale, confounded design, absent
reference populations, invalid counts, or insufficient replicates. Use declared
alternatives only: Salmon for an optional quantification path, limma-voom for
normalized values, edgeR quasi-likelihood for very small groups, DWLS/BayesPrism
when multi-donor methods are unsuitable, and a PNG or text fallback when an
optional renderer fails. Preserve the original failure and changed contract.

## Outputs

Return mode-tagged matrices/tables, QC figures, design and reference manifests,
fit artifacts, downstream handoff metadata, validation status, and a clear
statement of what the result cannot establish.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling](../_shared/failure_handling.md).

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`bulk-omics-clustering`](references/source_workflows/bulk-omics-clustering/WORKFLOW.md) — Cluster biological samples or features in normalized quantitative matrices, compare method assumptions, and validate cluster separation and stability.
- [`bulk-rnaseq-counts-to-de-deseq2`](references/source_workflows/bulk-rnaseq-counts-to-de-deseq2/WORKFLOW.md) — Perform DESeq2 differential-expression analysis on raw RNA-seq integer counts with explicit design, fold-change shrinkage, transformations, and quality control.
- [`deconvolution-bulk-rnaseq`](references/source_workflows/deconvolution-bulk-rnaseq/WORKFLOW.md) — Estimate cell-type proportions in bulk RNA-seq samples from an annotated single-cell reference and compare composition across groups or timepoints.
- [`rnaseq-fastq-to-counts`](references/source_workflows/rnaseq-fastq-to-counts/WORKFLOW.md) — Convert raw bulk RNA-seq reads into a differential-expression-ready integer gene-by-sample count matrix with QC, strandedness inference, gene metadata, and a load check, without running the differential-expression contrast.

<!-- END MANAGED: SOURCE WORKFLOWS -->
