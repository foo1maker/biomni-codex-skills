---
name: single-cell-rna-seq-analysis
description: Analyze single-cell RNA-seq data from raw or processed matrices, including framework-neutral QC, normalization, clustering and annotation, trajectory inference, public atlas queries, and disease or target-focused interpretation. Use when a user needs an auditable scRNA-seq workflow with explicit mode and donor-level boundaries.
---

# Single-cell RNA-seq analysis

## Purpose

Route a single-cell RNA-seq question to the correct mode and preserve the
distinction between cell-level processing, trajectory inference, public atlas
retrieval, and disease/target interpretation. Keep framework choices as
adapters; the scientific contract is the matrix, metadata, estimand, and
quality evidence.

## When to use

Use for raw or processed single-cell expression matrices, annotated objects,
trajectory questions, public atlas queries, or disease/target-focused cell
state analysis. Route bulk RNA-seq, spatial coordinates, chromatin-only data,
or clinical outcome modeling to their modality-specific skills.

## Inputs

- Declare one mode: `core`, `trajectory`, `atlas_query`, or
  `disease_target`.
- Core mode: raw counts or a processed AnnData/Seurat-like object, cell/sample
  metadata, species/build, and QC thresholds.
- Trajectory mode: processed object with dimensional reductions and cluster or
  state labels, a root definition, and optional spliced/unspliced layers.
- Atlas mode: organism, tissue/disease filters, release, donor/sample key,
  gene set, and comparison/aggregation plan.
- Disease/target mode: cell states, disease labels or contrasts, gene/evidence
  table, covariates, and a target-prioritization question.

## Workflow

1. Inspect matrix orientation, barcode/gene uniqueness, metadata joins,
   species/build, batch/donor structure, and raw-versus-processed status. Save
   an input manifest before changing the object.
2. In `core`, perform QC, filtering, normalization, feature selection,
   dimensional reduction, neighborhood graph construction, clustering,
   annotation, and marker testing. Keep thresholds and removed-cell counts.
3. In `trajectory`, require a processed object (at least 200 cells for the
   documented minimum), choose and justify the root, calculate connectivity
   and pseudotime, and optionally add velocity or fate probabilities only when
   their layers and packages are available.
4. In `atlas_query`, pin the public release and filters, retrieve data or
   metadata, aggregate at donor/sample level where possible, and run the
   pre-specified comparison. Do not treat a collection-level query as an
   independent biological replicate.
5. In `disease_target`, define the contrast and cell-state unit, integrate
   expression results with separately sourced genetic or target evidence, and
   label any prioritization as an inference. Export objects, tables, plots,
   diagnostics, and a provenance manifest.

## Resource selection

Choose optional adapters through the resource registry. Framework adapters may
include Scanpy/AnnData, Seurat, velocity or fate packages, public atlas
clients, and annotation references. Prefer user-supplied objects and pinned
public releases. If a client, reference, or package is unavailable, disclose
the missing adapter and use a compatible local object or synthetic fixture for
pipeline checks; never silently mix releases or species.

## Decision rules

- Keep `core`, `trajectory`, `atlas_query`, and `disease_target` outputs and
  estimands distinct. A trajectory is not a longitudinal patient study.
- Do not run trajectory inference before preprocessing or with too few cells;
  root choice, branching, velocity layers, and fate model are explicit inputs.
- Treat donor/sample as the replication unit for atlas or disease comparisons
  when the design permits; avoid pseudoreplication from cell counts.
- Keep Scanpy and Seurat implementation choices interchangeable at the contract
  level, while recording the actual adapter and version.
- Do not convert cell-state association or expression concordance into a
  disease-causal or therapeutic claim without independent evidence.

## Validation

Check cell/gene counts, QC distributions, doublet/ambient-RNA flags when
available, normalization and batch diagnostics, embedding and cluster
stability, marker specificity, donor balance, and multiple-testing handling.
For trajectory, validate root/terminal states, pseudotime coverage, branch
support, and velocity/fate confidence. For atlas/disease modes, verify release,
filters, donor aggregation, contrast direction, and missing-gene handling.

## Failure handling

- If raw/processed state, matrix orientation, metadata joins, species, or build
  is ambiguous, stop and request the missing contract.
- If QC removes nearly all cells or batch structure dominates, report the gate
  and return diagnostics before relaxing thresholds.
- If trajectory prerequisites or velocity/fate layers are missing, run only the
  supported core mode and label optional outputs unavailable.
- If atlas access fails, return the reproducible query and use an authorized
  local release or synthetic fixture; do not invent records.
- If donor coverage or cell-state counts are insufficient, suppress formal
  comparisons and return descriptive summaries with uncertainty.

## Outputs

Return a mode-labelled processed object, QC and filtering tables, embeddings,
cluster/marker results, trajectory or atlas/disease tables where supported,
figures, run parameters, resource release/access records, and explicit
limitations. Distinguish database observations, model predictions, analyses,
and biological inferences.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling](../_shared/failure_handling.md) to every mode.

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`scrna-disease-drug-discovery`](references/source_workflows/scrna-disease-drug-discovery/WORKFLOW.md) — Identify and prioritize disease-relevant drug targets from single-cell RNA-seq by integrating cell-type-resolved differential expression, pathways, ligand-receptor interactions, genetic evidence, and druggability.
- [`scrna-trajectory-inference`](references/source_workflows/scrna-trajectory-inference/WORKFLOW.md) — Order preprocessed single-cell RNA-seq cells along differentiation or disease trajectories, identify branches and terminal fates, discover transition-associated genes, and optionally estimate RNA velocity and fate probabilities.
- [`scrnaseq-scanpy-core-analysis`](references/source_workflows/scrnaseq-scanpy-core-analysis/WORKFLOW.md) — Process single-cell RNA-seq data through quality control, normalization, clustering, cell-type annotation, and analysis-ready visualization using Scanpy and the scverse ecosystem.
- [`scrnaseq-seurat-core-analysis`](references/source_workflows/scrnaseq-seurat-core-analysis/WORKFLOW.md) — Process single-cell RNA-seq data through quality control, normalization, clustering, cell-type annotation, and visualization using Seurat v5.
- [`single-cell-census-query`](references/source_workflows/single-cell-census-query/WORKFLOW.md) — Query the CZ CELLxGENE Census for gene-panel expression across cell types and perform donor-level pseudobulk differential expression for a case-versus-control comparison.

<!-- END MANAGED: SOURCE WORKFLOWS -->
