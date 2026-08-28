---
name: functional-genomics-perturbation-analysis
description: "Use when interpreting pooled perturbation screens with single-cell readouts or benchmarking predicted perturbation responses against held-out measurements."
---

# Functional genomics perturbation analysis

## Purpose

Route two distinct modes: (A) pooled perturbation screens with single-cell
transcriptional readout and (B) virtual perturbation model benchmarking. Share
replicate/QC/provenance discipline, but never treat a model prediction as an
experimental screen or merge their endpoints.

## When to use

- **Screen mode:** 10X/feature-barcode or compatible matrices with sgRNA-to-cell
  assignments, controls, and biological replicates.
- **Benchmark mode:** measured held-out perturbations, deterministic split/seed,
  model or control baseline, and a declared generalization regime.

## Inputs

- Screen mode: per-library expression/guide matrices, single-guide mappings,
  screen type, controls, replicate/batch metadata, cell type, expected target
  direction, and analysis goal.
- Benchmark mode: compatible single-cell object/dataset, condition/covariate
  fields, gene vocabulary, perturbation lists, split seed, model/checkpoint
  provenance, and license status.
- Shared: organism/build, identifiers, software/model versions, compute limits,
  and output/evidence requirements.

## Workflow

### Screen mode

1. Load libraries, map sgRNAs, remove unresolved/doublet assignments, apply
   cell-type-specific QC, and retain library/batch labels.
2. Correct identifiers, filter/normalize within the declared contract, and
   quantify mapping retention and combined cells.
3. Run a fast per-perturbation screen against non-targeting controls. Call
   preliminary hits only after recording the screening denominator.
4. Validate target expression in the expected direction (down for repression,
   up for activation where applicable). Promote only validated hits.
5. For selected validated hits, run a batch-aware differential-expression model
   when the optional adapter exists; otherwise retain fast-screen results as a
   lower-rigor partial output.

### Benchmark mode

1. Verify dataset/weight license, split construction, perturbation-regime counts,
   gene vocabulary, and seed before model calls.
2. Persist predictions per model immediately. Include a no-change control-mean
   baseline and compare learned models against it.
3. Compute absolute-expression and change-from-control metrics, top
   differential-expression direction agreement, and regime-stratified counts.
4. Report vocabulary coverage and distinguish training curves from absent
   training artifacts. Verify quantitative citations against retrieved records.

## Resource selection

- Screen adapters: registry-recorded `scanpy and anndata`, `pandas, NumPy, and SciPy`, `scikit-learn`, `diffxpy`, and optional `glmGamPoi with rpy2`.
- Benchmark adapters: `GEARS perturbation datasets`, `GEARS`, `scGPT`, and
  `Control-mean baseline`, each with checkpoint/dataset and license provenance.
- `primary` is the user-supplied measured data and declared endpoint. A CPU-
  feasible control baseline is a `fallback` for unavailable model execution,
  not equivalent model evidence.

See [resource selection policy](../_shared/resource_selection.md). Never assume
that a named package, checkpoint, or dataset is installed or redistributable.

## Decision rules

- **FGP-1:** Screen mode is limited to pooled screens with single-cell
  transcriptional readout and guide assignments; arrayed or non-transcriptional
  assays are outside this route.
- **FGP-2:** Use cell-type-specific QC; investigate mapping below 30%, doublets
  above 10%, validation below 50%, or inconsistent replicates.
- **FGP-3:** Preliminary screen hits are not validated hits until target-effect
  direction and replicate evidence pass.
- **FGP-4:** Benchmark mode prioritizes change-from-control and top-direction
  metrics; report the control baseline's all-gene direction artifact separately.
- **FGP-5:** Stratify benchmark results by generalization regime and label tiny
  regimes illustrative. Record gene-vocabulary coverage.
- **FGP-6:** Do not train or score with unknown/restricted data or weights until
  access/license is resolved.

## Validation

- Screen: verify mapping retention, singlet/doublet and cell-type QC counts,
  control definition, replicate balance, target-direction effect, and hit
  denominators. Preserve fast and rigorous outputs.
- Benchmark: assert split/seed/regime composition, held-out isolation,
  prediction persistence, baseline comparison, metric definitions, model
  vocabulary coverage, and citation verification.
- Apply [validation policy](../_shared/validation_policy.md) and classify model
  predictions, measured observations, and inferences separately with
  [evidence policy](../_shared/evidence_policy.md).

## Failure handling

If guide mapping or QC fails, preserve counts and stop promotion. Process large
libraries separately or use backed data only with disclosure. If the rigorous
screen adapter is missing, retain fast results as partial. If learned benchmark
models cannot run, use the control baseline and mark learned-model results
unavailable. Unknown licenses, split leakage, or missing held-out data are
blocking conditions.

See [failure handling policy](../_shared/failure_handling.md).

## Outputs

- Screen: mapping/QC ledger, normalized object, preliminary and validated hit
  tables, target-effect and replicate checks, optional rigorous DE, and figures.
- Benchmark: split manifest, per-model predictions, baseline and regime-stratified
  metrics, vocabulary coverage, model/checkpoint provenance, and limitations.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection policy](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling policy](../_shared/failure_handling.md).

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`pooled-crispr-screens`](references/source_workflows/pooled-crispr-screens/WORKFLOW.md) — Analyze pooled CRISPR screens with single-cell RNA-seq readout using a tiered workflow of fast screening, perturbation-target validation, and rigorous differential expression.
- [`virtual-cell-perturbation`](references/source_workflows/virtual-cell-perturbation/WORKFLOW.md) — Benchmark predicted single-cell transcriptional responses against held-out measured perturbations across datasets, models, and generalization regimes.

<!-- END MANAGED: SOURCE WORKFLOWS -->
