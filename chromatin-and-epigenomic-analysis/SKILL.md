---
name: chromatin-and-epigenomic-analysis
description: "Analyze differential chromatin regions, ChIP-Atlas peak enrichment, or single-cell ATAC/multiome data with build-aware QC, explicit region/coordinate provenance, confidence-gated annotation, and cautious interpretation. Use for differential peaks/DMRs, regulator enrichment near gene sets, or scATAC accessibility workflows; do not infer TF causality from enrichment alone."
---

# Chromatin and epigenomic analysis

## Purpose

Route three related modes while preserving their inputs and estimands:
differential peaks/DMRs, public regulator/peak enrichment near a gene set, and
single-cell ATAC/multiome QC and differential accessibility.

## When to use

| Mode | Minimum input |
|---|---|
| `differential-regions` | Two experiment groups, genome build, and `diffbind` or `dmr` analysis type |
| `peak-enrichment` | Gene symbols (≥3; 5–100 preferred), build, TSS windows, antigen/cell class, and peak threshold |
| `scatac-multiome` | Indexed fragments plus optional peak-by-barcode/multiome matrix and supported build |

## Inputs

- Differential mode: SRA/GEO experiment identifiers (at least two per group,
  three or more preferred), labels, assay type, build, and replicate design.
- Enrichment mode: gene list, genome build, antigen/cell class, peak stringency,
  TSS windows, and any independent coordinate-verification policy.
- scATAC mode: bgzip fragments and index, feature-barcode matrix when present,
  build, optional metadata, marker panel, and annotation confidence policy.

## Workflow

1. Classify the mode, validate identifiers/build/replicate structure, and record
   the input-to-region transformation.
2. In `differential-regions`, select `diffbind` for ChIP/ATAC/DNase peaks or
   `dmr` for Bisulfite-seq, preserve filtered and unfiltered regions, apply q
   value rules, and retain the group-A/group-B sign convention.
3. In `peak-enrichment`, map genes to regions using the declared coordinate
   source, submit the enrichment query, retain experiment-level results and
   aggregate factors separately, and compare submitted, mapped, and analyzed
   counts without inventing dropped genes.
4. In `scatac-multiome`, ingest fragments/call cells, calculate TSS enrichment,
   nucleosome signal, FRiP, blacklist ratio, and depth correlation; run TF-IDF
   and LSI, discard depth-dominated component 1, cluster, recall peaks per
   cluster, compute marker-panel gene activity, and test differential
   accessibility.
5. Export tables, raw/unfiltered artifacts, parameters, figures, checkpoints,
   build metadata, and confidence flags before interpretation.

## Resource selection

Prefer user-provided fragments/regions and matching genome resources. The
registry catalogs ChIP-Atlas, Ensembl, UCSC, CellMarker2, Signac/Seurat, MACS3,
and EnsDb/BSgenome adapters; inspect
`../../03_resource_registry/resource_registry.yaml` for access/license/version.
Adapters are optional and replaceable. Do not assume that a public enrichment
service, coordinate lookup, or annotation panel is reachable.

## Decision rules

- Use q-value <0.05 as the declared significance rule when no user threshold is
  supplied; keep effect size and replicate design visible.
- Filter non-standard contigs and regions shorter than 10 bp by default while
  retaining the unfiltered export. Flag dense one-directional or mitochondrial
  patterns as possible technical/structural artifacts under the documented
  assay-specific scope.
- Use median fold enrichment among significant experiments for factor strength;
  distinguish experiments from unique factors. For small gene sets, use
  exploratory language and sensitivity to single-gene changes.
- Match EnsDb, BSgenome, blacklist, and build. Remove LSI component 1, recall
  peaks by cluster, gate labels by confidence, and use Wilcoxon rather than a
  latent-variable logistic-regression differential-accessibility test.
- A nearby gene is proximity evidence, not proof of regulation or causality.

## Evidence

Distinguish public experiment/database observations, computed region statistics,
model-based enrichment, proximity annotations, and regulatory inferences. Keep
build, experiment identifiers, region coordinates, thresholds, and QC warnings
with every claim.

## Validation

- Input gate: IDs, build, assay/mode, group sizes, fragment/index integrity,
  coordinate mapping, and resource access/version.
- Intermediate gate: filtered/unfiltered counts, q-value/effect signs, dense
  region and peak-size QC, analyzed-region discrepancies, scATAC QC metrics,
  build match, LSI component handling, and annotation confidence.
- Output gate: every table/figure maps to parameters and source artifacts;
  missing BED, no-result, coordinate mismatch, PNG fallback, and unannotated
  regions remain visible.

## Failure handling

Stop on invalid IDs, build mismatch, corrupted fragments, insufficient groups,
or an inaccessible service. Keep complete statistics when coordinate annotation
fails, omit gene labels, and disclose it. Retain PNG when SVG fails. If no
regions or factors are significant, report the tested scope and QC rather than
claiming absence of regulation.

## Outputs

Return mode-tagged region/enrichment/accessibility tables, QC and figures,
unfiltered/raw artifacts where available, build and parameter manifest,
annotation confidence, evidence classification, and limitations.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling](../_shared/failure_handling.md).

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`chip-atlas-diff-analysis`](references/source_workflows/chip-atlas-diff-analysis/WORKFLOW.md) — Compare two groups of public experiments through ChIP-Atlas to identify differential peak regions or differentially methylated regions with effect sizes, significance statistics, QC warnings, plots, and tabular results.
- [`chip-atlas-peak-enrichment`](references/source_workflows/chip-atlas-peak-enrichment/WORKFLOW.md) — Identify public ChIP-seq experiments and aggregated factors whose peaks are enriched near an input gene set using the official ChIP-Atlas enrichment analysis, with significance, fold enrichment, overlap metrics, plots, and interpretation safeguards.
- [`scatac-multiome-analysis`](references/source_workflows/scatac-multiome-analysis/WORKFLOW.md) — Analyze fragments-first single-cell chromatin-accessibility data from QC through LSI, clustering, per-cluster peak recall, gene-activity annotation, and differential accessibility.

<!-- END MANAGED: SOURCE WORKFLOWS -->
