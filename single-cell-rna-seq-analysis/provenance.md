# Provenance

Five archived workflows were consolidated into a framework-neutral scRNA-seq
router. The modes and their boundaries are retained rather than merged into a
single undifferentiated pipeline.

| Source skill | Normalized record | Raw visible source | Source URL | Retrieved at | Sections and rules retained |
|---|---|---|---|---|---|
| `scrnaseq-scanpy-core-analysis` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_e4c50152a70f4d6fa8a4802573755f54?section=marketplace | 2026-08-14T12:18:00.215Z | `When to Use This Skill`; `Inputs`; `Standard Workflow`; `Step 1 — Load and QC | scripts/setup_and_import.py, scripts/qc_metrics.py, scripts/filter_cells.py`; `Step 2 — Normalize, reduce, integrate | scripts/normalize_data.py, scripts/scale_and_pca.py, scripts/integrate_scvi.py`; `Step 3 — Cluster, annotate, visualize | scripts/cluster_cells.py, scripts/find_markers.py, scripts/annotate_celltypes.py`; `Step 4 — Export results | scripts/export_results.py`; `Outputs`; `Common Issues`. Retains raw/processed distinction, QC, normalization, dimension reduction, clustering, annotation, and marker checks. |
| `scrnaseq-seurat-core-analysis` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_97fe00d84ed84cb784c5c4dc3e0478ab?section=marketplace | 2026-08-14T12:18:14.643Z | `When to Use This Skill`; `Inputs`; `Standard Workflow`; `Phase 1: QC and Filtering (Steps 1-3)`; `Phase 2: Normalization and Dimensionality Reduction (Steps 4-5)`; `Phase 3: Integration (Step 6 - Multi-Batch Only)`; `Phase 4: Clustering and Visualization (Steps 7-8)`; `Phase 5: Annotation and Differential Expression (Steps 9-10)`; `Phase 6: Export Results (Step 11)`; `Outputs`; `Common Issues`. Retains equivalent framework-neutral core contract and records implementation adapter/version. |
| `scrna-trajectory-inference` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_9971a7d3e3134ee9acc25ba3d0e1fdae?section=marketplace | 2026-08-14T12:17:32.21Z | `When to Use This Skill`; `Inputs`; `Outputs`; `Clarification Questions`; `Standard Workflow`; `Common Issues`; `Suggested Next Steps`. Retains processed-object prerequisite, 200-cell gate, root/branch choices, pseudotime, optional velocity/fate layers, and trajectory diagnostics. |
| `scrna-disease-drug-discovery` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_d7c1ec9d98a54a4eb7ae69a23c8314fe?section=marketplace | 2026-08-14T12:17:21.478Z | `When to Use This Skill`; `Inputs`; `Standard Workflow`; `Target Scoring Methodology`; `Data Integrity Rules`; `Decision Guide`; `Common Issues`; `References`. Retains disease/target cell-state contrasts, genetic/evidence integration, cell-level versus donor-level caution, and non-causal prioritization. |
| `single-cell-census-query` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_99b62aa3fd5ef6688b599c2a76b92313?section=marketplace | 2026-08-14T12:20:54.632Z | `Scope`; `Inputs (parameters — replace the placeholders, do not hard-code biology)`; `Workflow`; `Step 1 — Resolve inputs & scope`; `Step 2 — Pre-flight label verification (mandatory)`; `Step 3 — Expression atlas query`; `Step 4 — Build donor × cell_type pseudobulk (raw counts)`; `Step 5 — Pseudobulk DESeq2 DE`; `Scientific caveats (state the relevant ones in the report)`; `Compute guidance (size from evidence, not intuition)`; `CRITICAL pre-flight: verify labels BEFORE analysis (missing-label protocol)`; `Reference files`. Retains release-pinned atlas filters, metadata/expression retrieval, donor/sample aggregation, and query provenance. |

## Rule-to-source map

- Framework-neutral core processing and QC -> `scrnaseq-scanpy-core-analysis`
  and `scrnaseq-seurat-core-analysis`.
- Processed-object trajectory, root/branch, velocity, and fate gates ->
  `scrna-trajectory-inference`.
- Disease/target interpretation and evidence integration ->
  `scrna-disease-drug-discovery`.
- Public atlas query, release pinning, and donor-level comparison ->
  `single-cell-census-query`.

URLs and capture timestamps are copied exactly from the normalized `source`
records. Adapter availability and terms must be checked at execution time.
