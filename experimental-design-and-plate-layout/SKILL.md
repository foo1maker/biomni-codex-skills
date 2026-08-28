---
name: experimental-design-and-plate-layout
description: "Use when planning a two-group count-based experiment or a randomized 96/384-well layout with power, replication, batch/positional-bias, and export validation."
---

# Experimental design and plate layout

## Purpose

Plan sample size and power for the supported two-group count-based design, or
construct a spatially balanced plate layout with explicit controls and
replication. These are linked planning modes, not a general-purpose design or
downstream inferential analysis.

## When to use

- Count-based mode: independent two-group, one-to-one allocation with an
  analytic negative-binomial power model.
- Plate mode: 96- or 384-well experiments needing randomization, edge strategy,
  control distribution, and an auditable well map.

Route multifactor, paired, repeated-measures, interaction, time-course, unequal
allocation, survival, or simulation-based designs to a method that supports
their estimand.

## Inputs

- Mode, endpoint/assay, treatment groups, sample unit, effect size, alpha,
  target power, and available samples/plates.
- Count mode: pilot or literature biological variability, expected count, DE
  proportion, batch structure, covariates, and sequencing-depth assumptions.
- Plate mode: plate format, technical and biological replicate counts, positive/
  negative/blank controls, edge strategy, plate count, metadata, reserved wells,
  and optional ratiometric measurands/calibrator.
- Registry-recorded software/resource choices, versions, access/license status,
  and deterministic randomization seed.

## Workflow

1. Freeze the mode, estimand, replication vocabulary, inputs, and parameter
   provenance. Prefer measured pilot variability; otherwise label a literature
   coefficient of variation and sweep it.
2. Count mode: compute per-gene power, FDR-aware sample size, and sensitivity to
   CV, DE proportion, and expected per-gene count. Keep per-gene and experiment-
   wide power distinct.
3. Count mode: cross condition across batches, balance known covariates, check
   confounding, and preserve the complete design parameters.
4. Plate mode: keep biological preparations distinct from technical wells;
   define edge protection, controls, and any ratiometric block co-location.
5. Plate mode: generate a spatially balanced randomized layout with a recorded
   seed and method. Test quadrant, row, column, edge, plate, and co-location
   confounding.
6. Re-run power, sensitivity, and all layout checks after any seed/method or
   design change. Export tidy and plate-shaped tables, a well census, parameters,
   and visualizations.

## Resource selection

- `primary` count adapters: registry-recorded `DESeq2` for pilot count context,
  `RNASeqPower` for per-gene power, `RnaSeqSampleSize` for FDR-aware planning,
  and `anticlust` for covariate-balanced batch assignment.
- `secondary`: `IHW`, `pwr`, or a registry-recorded plate-layout optimizer and
  map renderer when their contract, version, and license are known.
- `fallback`: a CSV-only layout export when workbook export is unavailable, or
  a different seed/method when layout quality or confounding checks fail. Keep
  the failure and changed output visible.

Use [resource selection policy](../_shared/resource_selection.md). A named
package from the source snapshot that is absent from the registry is
`UNKNOWN`, not an instruction to install it.

## Decision rules

- **EXP-1:** Use an FDR-aware sample-size estimate for the plan; do not present
  per-gene power as experiment-wide discovery power.
- **EXP-2:** Batch must cross condition. Balancing known covariates cannot repair
  condition-covariate confounding.
- **EXP-3:** Prefer pilot biological CV; disclose literature CV source and run
  sensitivity over CV, DE proportion, and expected count.
- **EXP-4:** Treat technical wells as measurement precision and biological
  preparations as the independent unit; never substitute technical replication.
- **EXP-5:** For plate edge risk choose `controls_only`, `empty`, or `include`
  explicitly. For ratiometric assays keep a biological sample block together
  and place the inter-plate calibrator on every plate.
- **EXP-6:** A layout-quality score below 80% triggers another seed, more
  optimization, or another method; the heuristic is not publication evidence.
- **EXP-7:** If biological power is under 0.80 or biological SD is absent, stop
  and present redesign/accept-underpowering choices rather than overclaiming.

## Validation

- Verify endpoint, assay, allocation, CV source, expected count, alpha, FDR
  method, sample count, and sensitivity range.
- Check condition-by-batch/covariate balance, layout score, positional
  confounding, control distribution, co-location, and complete well census.
- Confirm exported parameters equal computed recommendations and every table or
  map is non-empty. Tag values as user-provided, example, function-default, or
  inferred.

See [validation policy](../_shared/validation_policy.md) and
[evidence policy](../_shared/evidence_policy.md).

## Failure handling

Block unsupported design modes, confounding, missing biological variability,
or inconsistent exports. If sample capacity is insufficient, disclose the
assumption change or redesign; do not silently lower power. If a workbook or
vector export fails, preserve CSV/PNG outputs and record the fallback. Any new
layout seed requires all dependent checks to be repeated.

See [failure handling policy](../_shared/failure_handling.md).

## Outputs

- Count mode: power/sample-size tables, sensitivity curves, batch design,
  statistical analysis plan, and machine-readable parameters.
- Plate mode: randomized tidy/grid layouts, well census, control/edge and
  confounding checks, power/sensitivity summary, seed/method record, and
  CSV/optional workbook/figure outputs.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection policy](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling policy](../_shared/failure_handling.md).

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`experimental-design-statistics`](references/source_workflows/experimental-design-statistics/WORKFLOW.md) — Plan two-group count-based omics experiments using analytic power and sample-size calculations, batch-balanced layouts, multiple-testing guidance, and sensitivity analysis.
- [`microplate-layout-design`](references/source_workflows/microplate-layout-design/WORKFLOW.md) — Design randomized 96-well or 384-well plate layouts that reduce positional bias, manage edge effects, balance treatment and covariates, distribute controls, distinguish technical from biological replication, and export auditable lab-ready maps.

<!-- END MANAGED: SOURCE WORKFLOWS -->
