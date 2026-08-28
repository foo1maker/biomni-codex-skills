---
name: immune-repertoire-and-cytometry-analysis
description: "Use when AIRR repertoire data or flow/mass-cytometry measurements need modality-aware QC, descriptive phenotyping, annotation validation, or replication-gated abundance analysis."
---

# Immune repertoire and cytometry analysis

## Purpose

Route two modes: (A) descriptive TCR/BCR repertoire profiling and (B)
flow/mass-cytometry processing. Both require modality-aware QC and auditable
metadata, but clonotype statistics and cytometry population inference remain
separate endpoints.

## When to use

- AIRR mode: called-repertoire files or compatible per-sample outputs are
  available; raw-read assembly is outside scope.
- Cytometry mode: FCS files, sample metadata, and optional compensation/manual
  gate material are available.

## Inputs

- AIRR: one repertoire file/folder per sample, sample metadata, receptor/chain/
  modality/species, and optional group definition.
- Cytometry: FCS files, acquisition modality, sample metadata, compensation/FMO/
  spillover controls, thresholds/templates, and optional manual labels.
- Shared: provenance/access/license, output scope, replicate unit, and any
  marker/annotation resource.

## Workflow

### AIRR mode

1. Confirm input format and sample depth; make receptor, chain, modality, and
   group definitions explicit, allowing expert overrides.
2. Compute clonality, diversity, rarefaction, V/J usage, and pairwise overlap.
   Interpret single-cell abundance as cells and bulk abundance as templates or
   reads, with depth normalization before richness comparisons.
3. Flag unusually high overlap as a sample-provenance consistency signal.
4. Run two-sided group tests only for a declared two-level contrast; label the
   comparison exploratory when the smaller group has fewer than four samples.
5. Add antigen annotation only when an optional database resolves the sequence;
   retain the omission rather than fabricating calls.

### Cytometry mode

1. Load FCS, identify modality, apply appropriate compensation/transformation,
   and perform first-class QC and pre-gating.
2. Cluster post-QC cells, assign marker-based labels, and refine annotations.
3. Quantify populations and embeddings using all post-QC cells; use balanced
   subsampling only for visualization.
4. When manual gates exist, reconcile identifiers and validate automated
   abundances against them.
5. Run differential abundance only with at least two groups and three samples
   per group; otherwise return descriptive population summaries.

## Resource selection

- AIRR adapters: registry-recorded `immunarch 0.9.1`; `McPAS-TCR` and `VDJdb`
  are optional annotation adapters and never create unsupported antigen calls.
- Cytometry adapters: `flowCore`, `CATALYST`, `FlowSOM`, `diffcyt`, `CellMarker2`,
  and `HDCytoData`, selected only when their input contract and version match.
- `primary` is the user-provided measurement/repertoire data. Manual gates are
  an independent validation resource, not a replacement for QC.

See [resource selection policy](../_shared/resource_selection.md). Unknown
resource versions or license/access status are recorded as such.

## Decision rules

- **IRC-1:** For single-cell repertoire data do not use Chao1 as a richness
  estimate; emphasize evenness and rarefaction. Depth-normalize bulk data.
- **IRC-2:** Do not equate exact BCR CDR3 matches with clonal lineages when
  somatic-hypermutation lineage structure is not modeled.
- **IRC-3:** High repertoire overlap triggers donor/replicate/contamination
  provenance review, not an antigen-biology claim.
- **IRC-4:** Cytometry QC precedes clustering/annotation; if compensation is
  singular, mark the data uncompensated and retain the warning.
- **IRC-5:** Differential abundance requires the declared replication threshold;
  otherwise return descriptive abundance only.
- **IRC-6:** Manual-gate reconciliation is mandatory when a manual export exists;
  report agreement metrics rather than only automated self-consistency.

## Validation

- AIRR: verify sample depth, rarefaction, receptor/chain/modality, abundance
  semantics, overlap flags, and group-size labels.
- Cytometry: preserve compensation status, transformation, pre-gating thresholds,
  event-retention counts, cluster/marker definitions, manual reconciliation, and
  replication gate.
- Distinguish measured abundances, model-derived clusters, annotations, and
  exploratory group inferences with [evidence policy](../_shared/evidence_policy.md).

## Failure handling

If repertoire calling is absent, stop before raw-read assembly. If antigen
resources are unavailable, skip annotation and disclose it. If QC is too
aggressive or compensation fails, retain the status and warnings. Under-
replicated cytometry returns descriptive results only. Preserve all excluded
events, unresolved labels, and access gaps.

See [failure handling policy](../_shared/failure_handling.md).

## Outputs

- AIRR: sample/depth manifest, clonality/diversity/rarefaction/VJ/overlap tables,
  optional exploratory tests, and provenance-aware figures.
- Cytometry: QC log, threshold templates, annotated object, population/embedding
  tables, manual-validation and differential-abundance tables when eligible.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection policy](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling policy](../_shared/failure_handling.md).

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`flow-cytometry-analysis`](references/source_workflows/flow-cytometry-analysis/WORKFLOW.md) — Process flow-cytometry or mass-cytometry data through modality-aware quality control, clustering, cell-population annotation, quantification, optional manual-gate benchmarking, and conditional differential-abundance analysis.
- [`immune-repertoire-airr`](references/source_workflows/immune-repertoire-airr/WORKFLOW.md) — Perform descriptive TCR or BCR repertoire profiling across clonality, diversity, V/J gene usage, and pairwise overlap, with interpretation adapted to single-cell versus bulk data.

<!-- END MANAGED: SOURCE WORKFLOWS -->
