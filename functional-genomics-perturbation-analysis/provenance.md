# Provenance

This skill consolidates two candidate modes while retaining their distinct
inputs, endpoints, and interpretation limits. Platform compute, runtime paths,
packaged scripts, and branded report calls were excluded.

## Source records

| source skill | normalized path | raw source path | source URL | retrieved_at |
|---|---|---|---|---|
| `pooled-crispr-screens` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_d3fa7c2c0f2b43ffb679e44ad7a3eafe?section=marketplace | 2026-08-14T12:15:47.573Z |
| `virtual-cell-perturbation` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_ee05eb5df2ca51cf172af24e65f59022?section=marketplace | 2026-08-14T12:23:40.502Z |

## Rule and mode mapping

| id | distilled rule or mode | source skill | source_section | source URL | retrieved_at |
|---|---|---|---|---|---|
| FGP-1 | Scope screen mode to pooled single-cell transcriptional readouts with guide assignments. | `pooled-crispr-screens` | `When to Use This Skill`; `Common Issues` | https://biomni.phylo.bio/skills/skill_d3fa7c2c0f2b43ffb679e44ad7a3eafe?section=marketplace | 2026-08-14T12:15:47.573Z |
| FGP-2 | Use cell-type-specific QC and investigate mapping <30%, doublets >10%, validation <50%, or replicate disagreement. | `pooled-crispr-screens` | `Common Issues`; `Standard Workflow` | https://biomni.phylo.bio/skills/skill_d3fa7c2c0f2b43ffb679e44ad7a3eafe?section=marketplace | 2026-08-14T12:15:47.573Z |
| FGP-3 | Require target-effect validation before promoting a screen hit. | `pooled-crispr-screens` | `Standard Workflow` | https://biomni.phylo.bio/skills/skill_d3fa7c2c0f2b43ffb679e44ad7a3eafe?section=marketplace | 2026-08-14T12:15:47.573Z |
| FGP-4 | Compare virtual perturbation models with change-from-control and direction metrics plus a control baseline. | `virtual-cell-perturbation` | `Metrics & scientific caveats`; `Step 4 — Metrics vs. held-out DE + baseline` | https://biomni.phylo.bio/skills/skill_ee05eb5df2ca51cf172af24e65f59022?section=marketplace | 2026-08-14T12:23:40.502Z |
| FGP-5 | Record deterministic split/regime counts, vocabulary coverage, and tiny-regime warnings. | `virtual-cell-perturbation` | `Step 1 — Data + split`; `Step 4 — Metrics vs. held-out DE + baseline` | https://biomni.phylo.bio/skills/skill_ee05eb5df2ca51cf172af24e65f59022?section=marketplace | 2026-08-14T12:23:40.502Z |
| FGP-6 | Use only open or commercially usable data and weights; flag unknown/restricted licensing. | `virtual-cell-perturbation` | `Scope`; `Metrics & scientific caveats` | https://biomni.phylo.bio/skills/skill_ee05eb5df2ca51cf172af24e65f59022?section=marketplace | 2026-08-14T12:23:40.502Z |
| FGP-M1 | Mapping/QC failures, memory limits, missing rigorous adapter, unknown weights, or unavailable learned models. | both sources | `Common Issues`; `Scope`; `Metrics & scientific caveats` | https://biomni.phylo.bio/skills/skill_d3fa7c2c0f2b43ffb679e44ad7a3eafe?section=marketplace; https://biomni.phylo.bio/skills/skill_ee05eb5df2ca51cf172af24e65f59022?section=marketplace | 2026-08-14T12:15:47.573Z; 2026-08-14T12:23:40.502Z |

## Resource provenance

| registry/resource key | source skill | source_section | source URL | retrieved_at | role |
|---|---|---|---|---|---|
| `scanpy and anndata`; `pandas, NumPy, and SciPy`; `scikit-learn`; `diffxpy`; `glmGamPoi with rpy2` | `pooled-crispr-screens` | `Installation` | https://biomni.phylo.bio/skills/skill_d3fa7c2c0f2b43ffb679e44ad7a3eafe?section=marketplace | 2026-08-14T12:15:47.573Z | screen data/QC/statistics adapters |
| `GEARS perturbation datasets`; `GEARS`; `scGPT`; `Control-mean baseline` | `virtual-cell-perturbation` | `Scope`; `Step 2 — Get weights (scGPT only; choose ONE path)`; `Virtual Cell Perturbation Benchmark` | https://biomni.phylo.bio/skills/skill_ee05eb5df2ca51cf172af24e65f59022?section=marketplace | 2026-08-14T12:23:40.502Z | benchmark dataset/model/baseline adapters |

