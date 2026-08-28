# Provenance

This skill retains two modes from separate archived source skills. Screen/hit
analysis and product-unit QC are not merged at the data or estimand level.

## Archive inputs

| Source skill | Normalized record | Raw visible source | Raw metadata |
|---|---|---|---|
| cell-therapy-car-design | `../../02_normalized/cell-therapy-car-design.yaml` | `../../00_raw/cell-therapy-car-design/source.md` | `../../00_raw/cell-therapy-car-design/metadata.json` |
| cell-therapy-qc-scorecard | `../../02_normalized/cell-therapy-qc-scorecard.yaml` | `../../00_raw/cell-therapy-qc-scorecard/source.md` | `../../00_raw/cell-therapy-qc-scorecard/metadata.json` |

## Derived-rule mappings

| Derived mode/rule | Source skill | Visible source section | Source URL | Retrieved at |
|---|---|---|---|---|
| Confirm phenotype direction, donor structure, and screen contrast from retrieved evidence | cell-therapy-car-design | `When to use this skill`; `Part 1 — CRISPR screen reanalysis + hit validation` | https://biomni.phylo.bio/skills/skill_1a2d4c938cf0cd29657bf065517bca3a?section=marketplace | 2026-08-14T12:18:24.941Z |
| Reconstruct a missing guide table transparently from read structure and a reference library | cell-therapy-car-design | `Part 1 — CRISPR screen reanalysis + hit validation`; `Critical decision points (ask the user if unclear)` | https://biomni.phylo.bio/skills/skill_1a2d4c938cf0cd29657bf065517bca3a?section=marketplace | 2026-08-14T12:18:24.941Z |
| Keep control normalization, sensitivity normalization, and broad-essentiality checks separate | cell-therapy-car-design | `Part 1 — CRISPR screen reanalysis + hit validation`; `Honest limitations to state in any report` | https://biomni.phylo.bio/skills/skill_1a2d4c938cf0cd29657bf065517bca3a?section=marketplace | 2026-08-14T12:18:24.941Z |
| Use validated parts and verify translated identity after construct optimization | cell-therapy-car-design | `Part 2 — CAR design workflow`; `Native-first principle (read this first)` | https://biomni.phylo.bio/skills/skill_1a2d4c938cf0cd29657bf065517bca3a?section=marketplace | 2026-08-14T12:18:24.941Z |
| Activate QC modules conditionally and aggregate by the worst active module | cell-therapy-qc-scorecard | `Adaptive Module Set`; `Standard Workflow`; `What Makes This Different From Generic scRNA-seq QC` | https://biomni.phylo.bio/skills/skill_30aae8fada7b78e30f7275374cebbe68?section=marketplace | 2026-08-14T12:16:02.578Z |
| Use raw target anchors, specificity-gated residual calls, and target-negative off-target calls | cell-therapy-qc-scorecard | `Standard Workflow`; `Scientific Caveats (read before trusting or editing)` | https://biomni.phylo.bio/skills/skill_30aae8fada7b78e30f7275374cebbe68?section=marketplace | 2026-08-14T12:16:02.578Z |
| Preserve GREEN/AMBER/RED thresholds, overrides, detection limits, and threshold provenance | cell-therapy-qc-scorecard | `Configuration Schema`; `Outputs`; `References (methodology & provenance)` | https://biomni.phylo.bio/skills/skill_30aae8fada7b78e30f7275374cebbe68?section=marketplace | 2026-08-14T12:16:02.578Z |
| Phrase rare-event non-detection as below detection and recommend an orthogonal assay | cell-therapy-qc-scorecard | `Scientific Caveats (read before trusting or editing)`; `Outputs` | https://biomni.phylo.bio/skills/skill_30aae8fada7b78e30f7275374cebbe68?section=marketplace | 2026-08-14T12:16:02.578Z |

## Resource and exclusion notes

The catalog authority is `../../03_resource_registry/resource_registry.yaml`
(registry version `2026-08-14`, 1,161 final unique records). GEO, DepMap,
Addgene, RCSB PDB, CellMarker2, and software names are replaceable adapters;
check access and licensing before use. Source-local platform orchestration,
brand packaging, and fixed paths were intentionally excluded.
