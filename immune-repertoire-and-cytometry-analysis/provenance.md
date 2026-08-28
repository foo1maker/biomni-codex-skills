# Provenance

This skill preserves the AIRR and cytometry mode boundaries. Platform compute,
internal paths, packaged entry points, and branded/report-only components were
excluded.

## Source records

| source skill | normalized path | raw source path | source URL | retrieved_at |
|---|---|---|---|---|
| `immune-repertoire-airr` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_d1c7d4c4273d38f002903e8199b4085c?section=marketplace | 2026-08-14T12:19:04.117Z |
| `flow-cytometry-analysis` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_dfc4fd9cdc6b7fba7c880cc7f87ecd7f?section=marketplace | 2026-08-14T12:17:15.455Z |

## Rule and mode mapping

| id | distilled rule or mode | source skill | source_section | source URL | retrieved_at |
|---|---|---|---|---|---|
| IRC-1 | Use evenness/rarefaction for single-cell repertoire data and depth-normalize bulk comparisons. | `immune-repertoire-airr` | `Workflow`; `Scientific caveats` | https://biomni.phylo.bio/skills/skill_d1c7d4c4273d38f002903e8199b4085c?section=marketplace | 2026-08-14T12:19:04.117Z |
| IRC-2 | Do not infer BCR lineages from exact CDR3 matches when hypermutation is not modeled. | `immune-repertoire-airr` | `Scientific caveats` | https://biomni.phylo.bio/skills/skill_d1c7d4c4273d38f002903e8199b4085c?section=marketplace | 2026-08-14T12:19:04.117Z |
| IRC-3 | Treat high overlap as provenance review signal. | `immune-repertoire-airr` | `Workflow`; `Scientific caveats` | https://biomni.phylo.bio/skills/skill_d1c7d4c4273d38f002903e8199b4085c?section=marketplace | 2026-08-14T12:19:04.117Z |
| IRC-4 | QC and pre-gating precede cytometry analysis; singular compensation remains explicitly uncompensated. | `flow-cytometry-analysis` | `QC / pre-gating (do this — real FCS is not pre-cleaned)`; `Scientific caveats` | https://biomni.phylo.bio/skills/skill_dfc4fd9cdc6b7fba7c880cc7f87ecd7f?section=marketplace | 2026-08-14T12:17:15.455Z |
| IRC-5 | Require at least three samples per group and two groups for differential abundance. | `flow-cytometry-analysis` | `6. Differential abundance (conditional, strict) — scripts/06_diff_abundance.R` | https://biomni.phylo.bio/skills/skill_dfc4fd9cdc6b7fba7c880cc7f87ecd7f?section=marketplace | 2026-08-14T12:17:15.455Z |
| IRC-6 | Validate automated populations against manual gates when available. | `flow-cytometry-analysis` | `4b. Validate vs a manual-gating export (MANDATORY when one exists) — scripts/08_validate_vs_manual.R` | https://biomni.phylo.bio/skills/skill_dfc4fd9cdc6b7fba7c880cc7f87ecd7f?section=marketplace | 2026-08-14T12:17:15.455Z |
| IRC-M1 | Shallow/unequal depth, high overlap, invalid compensation, aggressive pre-gating, or under-replication. | both sources | `Scientific caveats` | https://biomni.phylo.bio/skills/skill_d1c7d4c4273d38f002903e8199b4085c?section=marketplace; https://biomni.phylo.bio/skills/skill_dfc4fd9cdc6b7fba7c880cc7f87ecd7f?section=marketplace | 2026-08-14T12:19:04.117Z; 2026-08-14T12:17:15.455Z |

## Resource provenance

| registry/resource key | source skill | source_section | source URL | retrieved_at | role |
|---|---|---|---|---|---|
| `immunarch 0.9.1` | `immune-repertoire-airr` | `Environment / installation (READ THIS FIRST)` | https://biomni.phylo.bio/skills/skill_d1c7d4c4273d38f002903e8199b4085c?section=marketplace | 2026-08-14T12:19:04.117Z | repertoire loading/statistics adapter |
| `McPAS-TCR`; `VDJdb` | `immune-repertoire-airr` | `Optional enrichment (graceful-skip)` | https://biomni.phylo.bio/skills/skill_d1c7d4c4273d38f002903e8199b4085c?section=marketplace | 2026-08-14T12:19:04.117Z | optional annotation adapters |
| `flowCore`; `CATALYST`; `FlowSOM`; `diffcyt` | `flow-cytometry-analysis` | `Environment resources this skill uses` | https://biomni.phylo.bio/skills/skill_dfc4fd9cdc6b7fba7c880cc7f87ecd7f?section=marketplace | 2026-08-14T12:17:15.455Z | FCS/QC/clustering/differential-abundance adapters |
| `CellMarker2`; `HDCytoData` | `flow-cytometry-analysis` | `Environment resources this skill uses` | https://biomni.phylo.bio/skills/skill_dfc4fd9cdc6b7fba7c880cc7f87ecd7f?section=marketplace | 2026-08-14T12:17:15.455Z | optional marker/reference adapters |

