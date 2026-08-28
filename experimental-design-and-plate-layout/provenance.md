# Provenance

The derived skill combines the supported count-based power/planning mode and
the 96/384-well spatial-layout mode. Platform report generation, internal
script paths, and branded or tool-specific orchestration were excluded.

## Source records

| source skill | normalized path | raw source path | source URL | retrieved_at |
|---|---|---|---|---|
| `experimental-design-statistics` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_40063c54fad9439a9acd36c76a25983f?section=marketplace | 2026-08-14T12:16:50.505Z |
| `microplate-layout-design` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_ea62d879a7384219a92cf683a8b63013?section=marketplace | 2026-08-14T12:16:54.343Z |

## Rule and mode mapping

| id | distilled rule or mode | source skill | source_section | source URL | retrieved_at |
|---|---|---|---|---|---|
| EXP-1 | Keep per-gene and FDR-aware power distinct and use the FDR-aware estimate for planning. | `experimental-design-statistics` | `Step 2 - Calculate Design`; `Decision Guide` | https://biomni.phylo.bio/skills/skill_40063c54fad9439a9acd36c76a25983f?section=marketplace | 2026-08-14T12:16:50.505Z |
| EXP-2 | Batch must cross condition and confounding cannot be repaired after design. | `experimental-design-statistics` | `Decision Guide`; `Scientific Caveats` | https://biomni.phylo.bio/skills/skill_40063c54fad9439a9acd36c76a25983f?section=marketplace | 2026-08-14T12:16:50.505Z |
| EXP-3 | Prefer pilot biological CV and sweep CV, DE proportion, and expected count. | `experimental-design-statistics` | `Step 1 - Load Parameters`; `Step 2 - Calculate Design`; `Scientific Caveats` | https://biomni.phylo.bio/skills/skill_40063c54fad9439a9acd36c76a25983f?section=marketplace | 2026-08-14T12:16:50.505Z |
| EXP-4 | Separate biological from technical replication. | `microplate-layout-design` | `Replicate vocabulary: n_biological vs n_technical` | https://biomni.phylo.bio/skills/skill_ea62d879a7384219a92cf683a8b63013?section=marketplace | 2026-08-14T12:16:54.343Z |
| EXP-5 | Choose edge strategy and preserve ratiometric sample-block/calibrator co-location. | `microplate-layout-design` | `6. Edge Effect Strategy:`; `Ratiometric / multi-measurand designs (e.g. qPCR ΔΔCt)` | https://biomni.phylo.bio/skills/skill_ea62d879a7384219a92cf683a8b63013?section=marketplace | 2026-08-14T12:16:54.343Z |
| EXP-6 | Treat layout quality below 80% as a regeneration trigger, not evidence. | `microplate-layout-design` | `Step 2 - Generate Layout`; `Scientific Caveats` | https://biomni.phylo.bio/skills/skill_ea62d879a7384219a92cf683a8b63013?section=marketplace | 2026-08-14T12:16:54.343Z |
| EXP-7 | Stop or disclose choices when biological power is under 0.80 or SD is unavailable. | `microplate-layout-design` | `Step 2 - Generate Layout`; `Scientific Caveats` | https://biomni.phylo.bio/skills/skill_ea62d879a7384219a92cf683a8b63013?section=marketplace | 2026-08-14T12:16:54.343Z |
| EXP-M1 | Batch/condition confounding, literature CV mislabeling, or inconsistent export. | `experimental-design-statistics` | `Common Issues`; `Scientific Caveats` | https://biomni.phylo.bio/skills/skill_40063c54fad9439a9acd36c76a25983f?section=marketplace | 2026-08-14T12:16:50.505Z |
| EXP-M2 | Positional confounding, pseudoreplication, underpowering, or failed export. | `microplate-layout-design` | `Scientific Caveats`; `Step 2 - Generate Layout`; `Step 4 - Export Results` | https://biomni.phylo.bio/skills/skill_ea62d879a7384219a92cf683a8b63013?section=marketplace | 2026-08-14T12:16:54.343Z |

## Resource provenance

| registry/resource key | source skill | source_section | source URL | retrieved_at | role |
|---|---|---|---|---|---|
| `DESeq2` | `experimental-design-statistics` | `Installation` | https://biomni.phylo.bio/skills/skill_40063c54fad9439a9acd36c76a25983f?section=marketplace | 2026-08-14T12:16:50.505Z | pilot count context |
| `RNASeqPower`; `RnaSeqSampleSize` | `experimental-design-statistics` | `Installation`; `Step 2 - Calculate Design` | https://biomni.phylo.bio/skills/skill_40063c54fad9439a9acd36c76a25983f?section=marketplace | 2026-08-14T12:16:50.505Z | per-gene and FDR-aware power |
| `anticlust`; `IHW`; `pwr` | `experimental-design-statistics` | `Installation` | https://biomni.phylo.bio/skills/skill_40063c54fad9439a9acd36c76a25983f?section=marketplace | 2026-08-14T12:16:50.505Z | optional balance/multiple-testing/power adapters |
| `designit`; `ggplate`; `openxlsx` | `microplate-layout-design` | `Required Software`; `Optional (for enhanced output)` | https://biomni.phylo.bio/skills/skill_ea62d879a7384219a92cf683a8b63013?section=marketplace | 2026-08-14T12:16:54.343Z | use only if present in the current registry; otherwise `UNKNOWN` |
| `CSV-only export` | `microplate-layout-design` | `Step 4 - Export Results` | https://biomni.phylo.bio/skills/skill_ea62d879a7384219a92cf683a8b63013?section=marketplace | 2026-08-14T12:16:54.343Z | declared fallback when workbook export fails |

