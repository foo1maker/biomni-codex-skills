---
name: cell-therapy-design-and-qc
description: "Route two distinct cell-therapy workflows: pooled-screen-to-target and validated receptor-construct design, or single-cell product QC/release scorecards. Use for screen direction and hit QC, validated-part construct assembly, product identity/purity/contamination checks, conditional pluripotency or maturity modules, and GREEN/AMBER/RED calls; keep computational evidence separate from experimental release decisions."
---

# Cell-therapy design and QC

## Purpose

Handle a screen/construct-design mode and a product-QC scorecard mode under one
portable contract. They must remain separate because one studies perturbation
and construct hypotheses while the other evaluates product-unit measurements.

## When to use

| Mode | Use when | Required evidence |
|---|---|---|
| `screen-and-construct` | Reanalyze a pooled screen, nominate targets, or assemble a receptor construct | Screen design/contrast and validated part provenance; never reconstruct an unvalidated binding part |
| `product-qc-scorecard` | QC, characterize, or release-test single-cell product units | Per-unit single-cell input, target/product definition, species, marker-panel and threshold provenance |

## Inputs

- Screen/construct: public accession or reads, phenotype contrast, sample/donor
  design, non-targeting controls when present, validated binding-part source,
  domain/costimulatory choices, and a declared essentiality cross-check.
- Product QC: unit files or accession, product/target type, source class,
  species, engineering/transgene description, optional unit metadata, active
  modules, and threshold overrides.
- For both modes, record identifiers, source URLs, versions, and any access or
  license constraints before analysis.

## Workflow

1. Classify the mode; do not combine screen hit rankings with release calls.
2. In `screen-and-construct`, verify phenotype direction and donor structure
   from retrieved source evidence. Inspect read structure before guide recovery;
   if a guide table is missing, label a read-driven reconstruction. Normalize
   with non-targeting controls when available and retain a median-normalization
   sensitivity run.
3. Check mapping, zero-count guides, library evenness, control centering, and
   expected positive/negative controls. Cross-check hits against the declared
   broad-essentiality reference without presenting it as primary-cell proof.
4. Assemble only validated binding parts and declared domains; codon-optimize
   where requested, verify translated amino-acid identity, and emit domain and
   construct metadata. A computational construct remains a hypothesis.
5. In `product-qc-scorecard`, build a configuration, load every unit, standardize
   gene symbols, record raw cell counts, and calculate species fractions only
   for multi-species inputs.
6. Resolve target/off-target marker panels from a declared marker resource and
   product-specific evidence. Run technical QC, outlier/doublet filtering,
   normalization, and module flags.
7. Always run identity/purity, off-target-lineage, and technical QC. Activate
   residual-pluripotency only for the relevant source class and maturity only
   when a target-specific axis exists. Aggregate by the worst active module.
8. Export tables, exact panels and thresholds, unit calls, and provenance only
   after validating the rendered and machine-readable outputs.

## Resource selection

Use user-provided reads, validated parts, and product data when compatible. The
registry catalogs GEO, DepMap, Addgene, RCSB PDB, CellMarker2, Scanpy/Scrublet,
and related adapters; inspect
`../../03_resource_registry/resource_registry.yaml` for role, access, and
license status. These resources are replaceable and must not be assumed
available. A missing guide or marker source is a disclosed limitation, not
permission to invent one.

## Decision rules

- Confirm treatment/control direction from source evidence; reversal inverts
  screen hits. Treat positive-selection hits and essentiality flags as separate
  evidence axes.
- Never reconstruct a binding part; use a validated source and verify translated
  identity after any codon optimization.
- In QC, use raw-expression target anchors, target-negative gating for off-target
  cells, and specificity-gated pluripotency co-expression with a shuffled-null
  threshold.
- Default scorecard calls are guidance: identity/purity ≥90% GREEN, 75–90%
  AMBER, <75% RED; residual pluripotency <0.01% GREEN, 0.01–0.1% AMBER, >0.1%
  RED; off-target <2% GREEN, 2–10% AMBER, >10% RED; maturity ≥60% GREEN,
  40–60% AMBER, <40% RED when active. Record overrides and require product
  owner sign-off before treating them as release criteria.
- One RED active module makes the overall unit RED. A rare event not detected
  at the observed depth is `below_detection`, not zero.

## Evidence

Separate retrieved screen/product records, computed QC or ranking outputs,
construct hypotheses, release recommendations, and experimental claims. Keep
part provenance, unit/denominator, threshold version, and detection limits
attached to every material statement.

## Validation

- Input gate: mode, sample/unit metadata, phenotype direction or product
  definition, validated part/marker provenance, species, and thresholds.
- Intermediate gate: screen mapping/QC and normalization sensitivity; or unit
  filtering, doublet/species metrics, exact panels, per-cell flags, and module
  thresholds.
- Output gate: machine tables and figures agree; every call exposes panel,
  denominator, threshold, detection limit, and provenance. No computational call
  is reported as efficacy, potency, or regulatory release approval.

## Failure handling

Stop on reversed/unknown screen contrast, missing validated parts, unusable
reads, missing target markers, or incompatible single-cell input. Use explicit
fallbacks: read-driven guide reconstruction, median normalization without
invented controls, separate unit naming when metadata are absent, and inactive
maturity when no target axis exists. Preserve partial artifacts and label the
changed scope.

## Outputs

For screen mode, return reconstructed/normalized matrices, hit and control
tables, essentiality flags, construct sequences/annotations, and QC. For QC
mode, return per-unit calls, module tables, exact panels/thresholds, detection
limits, processed data references, and figures. In both modes include evidence
class, limitations, and provenance.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling](../_shared/failure_handling.md).

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`cell-therapy-car-design`](references/source_workflows/cell-therapy-car-design/WORKFLOW.md) — Combine pooled loss-of-function screen reanalysis and broad-essentiality filtering with construction of validated-scFv, second-generation CAR designs, producing both prioritized editing candidates and annotated receptor constructs.
- [`cell-therapy-qc-scorecard`](references/source_workflows/cell-therapy-qc-scorecard/WORKFLOW.md) — Convert single-cell RNA-seq measurements of a cell-therapy product into per-unit identity and safety module scores, GREEN/AMBER/RED module calls, and an overall release-oriented scorecard.

<!-- END MANAGED: SOURCE WORKFLOWS -->
