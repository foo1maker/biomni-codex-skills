---
name: regulatory-network-analysis
description: Analyze transcriptional regulation from single-cell expression, bulk differential expression, coexpression networks, or public TF-binding evidence. Use when a user needs regulons, modules, upstream-regulator ranking, or TF-target retrieval with explicit mode boundaries and non-causal interpretation.
---

# Regulatory network analysis

## Purpose

Route a regulatory question to the evidence-compatible mode and keep
coexpression, motif-supported regulons, public TF-target retrieval, and bulk
upstream-regulator integration as separate products. Treat all inferred
regulation as a hypothesis unless independently validated.

## When to use

Use for single-cell regulon activity, sample-level coexpression modules, TF
target queries from public binding experiments, or integration of bulk
differential expression with binding enrichment. Do not use de novo network
inference for a small bulk cohort, a histone-mark-only question, or a request
to prove causality from enrichment alone.

## Inputs

- Declare one mode: `single_cell_grn`, `coexpression`, `tf_target_query`, or
  `bulk_upstream_regulator`.
- Single-cell mode: filtered expression matrix, cell metadata, species and
  genome build, TF list, motif-ranking database, and motif annotations.
- Coexpression mode: normalized sample-by-gene matrix, sample traits, batch
  metadata, and a documented sample-size/feature policy.
- TF-target mode: case-sensitive TF symbol, assembly, TSS distance and query
  filters. Bulk mode: gene, log fold-change, adjusted p-value, genome/build,
  and enrichment thresholds.

## Workflow

1. Validate identifiers, species/build, matrix orientation, normalization,
   replicates, and the declared mode. Record the query and resource versions.
2. For `single_cell_grn`, filter low-quality cells/genes, infer coexpression
   adjacencies, prune with species-matched motifs, then score cell-level
   regulon activity. Preserve each stage separately.
3. For `coexpression`, select variable genes and soft power, construct modules,
   compress to eigengenes, correlate modules with traits, and rank hubs by
   membership/connectivity.
4. For `tf_target_query`, retrieve and rank public TF-bound targets with the
   requested assembly, distance, cell context, and evidence filters. Do not
   call this de novo inference.
5. For `bulk_upstream_regulator`, split up- and down-regulated genes, run
   enrichment independently, calculate overlap and directional concordance,
   then report the combined score as a heuristic ranking.

## Resource selection

Select optional adapters from the resource registry only after checking public
access, species/build coverage, version, and license status. Candidate adapters
include motif-ranking/annotation references, public TF-binding catalogs,
interaction evidence, and framework libraries. A user-supplied database or a
small documented reference fixture is preferable to an unavailable remote
resource. Never silently replace a missing reference with a different species
or genome build.

## Decision rules

- Keep the four modes distinct in inputs, statistical assumptions, and output
  labels; do not merge a public TF-target lookup with a de novo regulon.
- Require at least 500 cells and 2,000 expressed genes for de novo single-cell
  inference unless a new threshold is justified; treat fewer than 15 bulk
  samples as unstable for coexpression.
- Use TF-list identifiers and motif databases matching the expression species
  and build. For upstream analysis, classify direction only above the declared
  concordance threshold (60% is a source default); otherwise use `mixed`.
- Retain binding, overlap, and direction evidence separately. A hub, regulon,
  enrichment, or ranking is not a causal regulator claim.

## Validation

Verify cell/sample counts, gene/TF identifier overlap, matrix normalization,
reference integrity, module fit, regulon size, AUCell/activity distributions,
enrichment background, Fisher-test inputs, and direction calculations. Retain
raw adjacencies, motif-pruned regulons, module summaries, target tables, and
all ranking components. Validate important regulators against independent
literature, binding, or perturbation evidence.

## Failure handling

- If identifiers, species/build, motif files, or TF-target resources do not
  match, stop with a mapping report and do not guess.
- If memory or runtime is limiting, declare any subsampling or variable-gene
  restriction and preserve the changed scope; do not present it as full-data
  inference.
- If few genes pass directional filtering, report no stable regulator and only
  use a relaxed threshold when explicitly configured and disclosed.
- If public target retrieval fails, return the partial evidence and use a
  documented local/reference fallback; do not fabricate target genes.
- If activity or module scores are uniformly weak, report diagnostics before
  changing normalization or thresholds.

## Outputs

Produce mode-labelled machine-readable tables, intermediate evidence products,
activity/module matrices where applicable, target or regulator rankings with
component scores, diagnostics, figures, and a provenance manifest. Clearly
separate database observations, model outputs, inferred hypotheses, and any
recommendation.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling](../_shared/failure_handling.md) to every mode.

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`chip-atlas-target-genes`](references/source_workflows/chip-atlas-target-genes/WORKFLOW.md) — Retrieve and rank genes near peaks for a specified transcription factor from precomputed ChIP-Atlas public ChIP-seq matrices, with binding, coverage, interaction, cell-context, and co-location information.
- [`coexpression-network`](references/source_workflows/coexpression-network/WORKFLOW.md) — Build weighted gene co-expression networks to identify co-expressed modules, module-trait associations, and highly connected hub genes.
- [`grn-pyscenic`](references/source_workflows/grn-pyscenic/WORKFLOW.md) — Infer transcription-factor regulons de novo from single-cell RNA-seq expression and calculate cell-level regulon activity with the pySCENIC pipeline.
- [`upstream-regulator-analysis`](references/source_workflows/upstream-regulator-analysis/WORKFLOW.md) — Identify candidate transcriptional regulators by integrating ChIP-Atlas binding enrichment with bulk RNA-seq differential-expression results and directional target concordance.

<!-- END MANAGED: SOURCE WORKFLOWS -->
