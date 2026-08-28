# Provenance

Five source workflows support one portable small-molecule portfolio skill. The
four data-generating modes remain explicit so evidence is not silently pooled.

| Source skill | Normalized record | Raw visible source | Source URL | Retrieved at | Sections and rules retained |
|---|---|---|---|---|---|
| `adme-ml-modeling` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_9fd3e0632ea54e148757bb2469292ab8?section=marketplace | 2026-08-14T12:00:12.622Z | `Scope`; `Inputs`; `Workflow`; `Required outputs`; `Operating rules`; `Scientific caveats`; `Interpretation`; `Run configuration`; `Default demonstration (real public benchmark)`; `ADME ML modeling`. Retains labelled endpoint modeling, prospective evaluation, calibration, applicability-domain flags, and run provenance. |
| `binding-affinity-ml-model` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_5b530f17e1aaedd0139286faf1950c3c?section=marketplace | 2026-08-14T12:16:47.353Z | `Scope`; `Inputs`; `Workflow`; `Outputs (under <outdir>, default /mnt/results/qsar_run)`; `Requested modeling frameworks — NO SILENT SUBSTITUTION`; `Scientific caveats (carry these into any report)`; `Data sources & license (see references/DATA_SOURCES.md)`; `Environment`. Retains supervised affinity/potency curation, leakage-safe splits, model uncertainty, and requested/fallback framework provenance. |
| `drug-bioactivity-chembl` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_b14b08ae68081bbdaa898cc98affac2a?section=marketplace | 2026-08-14T12:20:46.737Z | `When to use`; `Inputs`; `Workflow`; `5. Curate with the "Standard" filter + provenance`; `6. Aggregate potency`; `7. Selectivity`; `9. Cellular activity (secondary, separate)`; `Error handling`; `Scientific caveats (read before reporting)`; `Data sources and licensing`; `References`. Retains measured potency/selectivity/cellular activity separation, exact-unit curation, record provenance, and release attribution. |
| `molecular-property-admet` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_c30c5d550c6b897400e363e8d1664a3c?section=marketplace | 2026-08-14T12:17:21.818Z | `When to Use This Skill`; `Inputs`; `Standard Workflow`; `Outputs`; `Common Issues`; `Clarification Questions`; `References`; `Suggested Next Steps`; `Percentile Reference Set (commercially permissive)`. Retains standardize-but-retain handling, descriptor/property endpoints, structural alerts, reference provenance, and license guardrails. |
| `off-target-safety-pharmacology` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_8c4bbb8b8155ca559167852f77ba44d1?section=marketplace | 2026-08-14T12:18:14.986Z | `Scope`; `Inputs`; `Pipeline overview`; `1. Resolve the compound — resolve_compound.py`; `2. Build the prediction panel — build_panel.py`; `4. Off-target predictors (two orthogonal engines)`; `5. Ground truth — fetch_ground_truth.py`; `6. Benchmark, agreement & tier gate — benchmark.py (THE HONESTY GATE)`; `Scientific caveats (keep these in the report)`; `Data sources & licenses`; `Optional gated stage (off by default): structure-level plausibility`. Retains measured/predicted off-target separation, primary-vs-adaptive panel provenance, data sufficiency, and non-clinical interpretation. |

## Rule-to-source map

- Labelled endpoint modeling, calibration, applicability domain ->
  `adme-ml-modeling.yaml`.
- Affinity/potency model splits, metrics, and fallback provenance ->
  `binding-affinity-ml-model.yaml`.
- Measured activity units, assay context, and attribution ->
  `drug-bioactivity-chembl.yaml`.
- Standardization, descriptor/property predictions, and license constraints ->
  `molecular-property-admet.yaml`.
- Core/adaptive panel separation and measured-versus-predicted safety ->
  `off-target-safety-pharmacology.yaml`.

URLs, timestamps, and raw paths are copied from normalized `source` records.
