---
name: spatial-transcriptomics-analysis
description: Analyze spatial transcriptomics count matrices with coordinates, images, and tissue metadata, including QC, normalization, domains, variable genes, differential expression, and neighborhood enrichment. Use when a user needs an auditable spatial workflow with platform assumptions and coordinate provenance made explicit.
---

# Spatial transcriptomics analysis

## Purpose

Analyze spatially indexed expression while keeping spot/cell resolution,
coordinate system, tissue image, and platform assumptions explicit. Produce
spatial domains and neighborhood evidence without implying single-cell
resolution when the assay does not support it.

## When to use

Use for Visium-style or comparable spatial transcriptomics data containing a
count matrix, spot coordinates, tissue metadata, and optionally an image.
Route ordinary scRNA-seq without coordinates, imaging-only data, or bulk
longitudinal data to their modality-specific workflows.

## Inputs

- Count matrix with feature identifiers and spot/cell identifiers.
- Spatial coordinates, tissue/spot inclusion mask, slide/section/sample IDs,
  and image plus scale factors when available.
- Species/build, annotation or region labels, batch metadata, and a declared
  contrast or neighborhood question.
- QC, normalization, domain-resolution, and multiple-testing configuration.

## Workflow

1. Validate matrix orientation, coordinate uniqueness, image alignment,
   tissue mask, sample/section keys, feature IDs, and reference/build. Record
   the raw object and coordinate/image provenance.
2. Apply spot/cell and feature QC, retain removed-row counts, normalize within a
   declared model, and assess library size, detected features, and spatially
   structured QC artifacts.
3. Compute variable features and dimension reduction, build a neighborhood
   graph, and identify spatial domains while preserving sample and section
   boundaries.
4. Test domain/region markers, spatially variable genes, and neighborhood
   enrichment under a pre-specified background and replicate unit. Use image
   information only when alignment and quality are verified.
5. Export an annotated object, coordinates/domains, QC and marker tables,
   spatial plots, parameters, and a provenance manifest.

## Resource selection

Resolve optional spatial-analysis libraries, annotation references, and
platform documentation through the resource registry. Prefer the user's
platform-native object and pinned image/scale metadata. If a library or image
is unavailable, run count-and-coordinate analyses that remain supported and
label image-dependent outputs unavailable. Do not silently apply a Visium
assumption to another platform.

## Decision rules

- Preserve the declared spot/cell resolution; do not call spot-level mixtures
  single-cell measurements without a validated deconvolution or segmentation
  method.
- Keep section/slide/sample as potential replication units and avoid treating
  neighboring spots as independent biological replicates.
- Define coordinate orientation, tissue mask, normalization, domain resolution,
  and spatial background before ranking genes or regions.
- Treat spatial association as evidence of localization or co-occurrence, not
  proof of cell-cell interaction or mechanism.

## Validation

Check coordinate/image alignment, tissue inclusion, library-size and feature
distributions, section/batch balance, domain stability, negative/positive
controls, spatial autocorrelation assumptions, replicate handling, and
multiple-testing correction. Verify every figure is derived from the current
coordinate table and that scale factors and platform metadata are preserved.

## Failure handling

- If coordinates, sample keys, tissue mask, or image scale cannot be mapped,
  stop the affected spatial branch and return a schema report.
- If QC is dominated by tissue or library artifacts, report diagnostics and
  avoid interpreting domains until the cause is resolved.
- If image alignment is unavailable, omit image overlays and state that only
  count/coordinate outputs were produced.
- If there are too few sections or replicates for formal inference, return
  descriptive spatial summaries and suppress overconfident p-values.
- If a platform adapter is unavailable, use a generic matrix/coordinate path
  only when its assumptions are documented.

## Outputs

Return an annotated spatial object, QC and filtering ledger, domain and marker
tables, spatially variable-gene and neighborhood results, figures with scale and
coordinate metadata, run configuration, resource records, and limitations.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling](../_shared/failure_handling.md) to every run.

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`spatial-transcriptomics`](references/source_workflows/spatial-transcriptomics/WORKFLOW.md) — Analyze 10x Visium spatial gene-expression data through quality control, spatial-domain clustering, spatially variable gene detection, and neighborhood analysis.

<!-- END MANAGED: SOURCE WORKFLOWS -->
