# Provenance

This derived skill consolidates differential-region, enrichment, and scATAC
workflows while retaining their mode-specific inputs and claims.

## Archive inputs

| Source skill | Normalized record | Raw visible source | Raw metadata |
|---|---|---|---|
| chip-atlas-diff-analysis | `../../02_normalized/chip-atlas-diff-analysis.yaml` | `../../00_raw/chip-atlas-diff-analysis/source.md` | `../../00_raw/chip-atlas-diff-analysis/metadata.json` |
| chip-atlas-peak-enrichment | `../../02_normalized/chip-atlas-peak-enrichment.yaml` | `../../00_raw/chip-atlas-peak-enrichment/source.md` | `../../00_raw/chip-atlas-peak-enrichment/metadata.json` |
| scatac-multiome-analysis | `../../02_normalized/scatac-multiome-analysis.yaml` | `../../00_raw/scatac-multiome-analysis/source.md` | `../../00_raw/scatac-multiome-analysis/metadata.json` |

## Derived-rule mappings

| Derived mode/rule | Source skill | Visible source section | Source URL | Retrieved at |
|---|---|---|---|---|
| Route diffbind for ChIP/ATAC/DNase peaks and dmr for Bisulfite-seq | chip-atlas-diff-analysis | `When to Use This Skill`; `Standard Workflow`; `Inputs` | https://biomni.phylo.bio/skills/skill_7f4da97222334fba9984eab5b78eb392?section=marketplace | 2026-08-14T12:17:54.632Z |
| Preserve filtered/unfiltered regions, q-value/effect signs, replicate design, and automated QC warnings | chip-atlas-diff-analysis | `Standard Workflow`; `Interpretation Guidelines`; `Outputs` | https://biomni.phylo.bio/skills/skill_7f4da97222334fba9984eab5b78eb392?section=marketplace | 2026-08-14T12:17:54.632Z |
| Treat nearest genes as proximity and retain PNG when vector rendering fails | chip-atlas-diff-analysis | `Interpretation Guidelines`; `Common Issues` | https://biomni.phylo.bio/skills/skill_7f4da97222334fba9984eab5b78eb392?section=marketplace | 2026-08-14T12:17:54.632Z |
| Require gene-list/build/window inputs and compare submitted, mapped, and analyzed region counts | chip-atlas-peak-enrichment | `Inputs`; `Standard Workflow`; `Outputs` | https://biomni.phylo.bio/skills/skill_dd3e525ed3e84cb9a73aa45781117110?section=marketplace | 2026-08-14T12:18:03.682Z |
| Use BH q-value as primary significance, fold enrichment as context, and separate experiments from factors | chip-atlas-peak-enrichment | `Interpretation Guidelines`; `Standard Workflow` | https://biomni.phylo.bio/skills/skill_dd3e525ed3e84cb9a73aa45781117110?section=marketplace | 2026-08-14T12:18:03.682Z |
| Frame small gene sets and study-availability bias as exploratory limitations | chip-atlas-peak-enrichment | `Interpretation Guidelines`; `Suggested Next Steps` | https://biomni.phylo.bio/skills/skill_dd3e525ed3e84cb9a73aa45781117110?section=marketplace | 2026-08-14T12:18:03.682Z |
| Perform fragments-first QC, TF-IDF/LSI with component-1 exclusion, and per-cluster peak recall | scatac-multiome-analysis | `Workflow`; `Scope`; `Scientific caveats (read before running)` | https://biomni.phylo.bio/skills/skill_206b4bf7080d00a289bb97143936de68?section=marketplace | 2026-08-14T12:17:11.689Z |
| Match genome resources and use confidence-gated marker/gene-activity annotation | scatac-multiome-analysis | `Workflow`; `Resource notes` | https://biomni.phylo.bio/skills/skill_206b4bf7080d00a289bb97143936de68?section=marketplace | 2026-08-14T12:17:11.689Z |
| Preserve build, package, session, accession, and seed provenance and use Wilcoxon differential accessibility | scatac-multiome-analysis | `Resource notes`; `Scientific caveats (read before running)` | https://biomni.phylo.bio/skills/skill_206b4bf7080d00a289bb97143936de68?section=marketplace | 2026-08-14T12:17:11.689Z |

## Resource and exclusion notes

The catalog authority is `../../03_resource_registry/resource_registry.yaml`
(registry version `2026-08-14`, 1,161 final unique records). ChIP-Atlas,
Ensembl, UCSC, CellMarker2, Signac/Seurat, MACS3, and genome packages are
replaceable adapters with run-specific access/license checks. Motif/chromVAR,
peak-to-gene linkage, trajectory, and cross-sample integration were not added
because the source contract excluded them.
