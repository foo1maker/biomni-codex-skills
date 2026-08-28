# Provenance

This skill consolidates four source modes while preserving their boundaries.
The cited records are the evidence authority; optional resource adapters are
not assumed to be installed or reachable.

| Source skill | Normalized record | Raw visible source | Source URL | Retrieved at | Sections and rules retained |
|---|---|---|---|---|---|
| `grn-pyscenic` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_35e45ff33cb141a981a3fc5711c4760f?section=marketplace | 2026-08-14T12:14:28.887Z | `When to Use This Skill`; `Inputs`; `Outputs`; `Standard Workflow`; `Clarification Questions`; `Common Issues`; `Suggested Next Steps`; `Installation`. Retains the three-stage adjacency/motif/activity chain, cell/gene gates, species-matched references, and orthogonal validation. |
| `chip-atlas-target-genes` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_aaafb560ba89480dbb01b2979f49630d?section=marketplace | 2026-08-14T12:18:14.415Z | `When to Use This Skill`; `Inputs`; `Outputs`; `Standard Workflow`; `Clarification Questions`; `Common Issues`; `Interpretation Guidelines`. Retains public TF-target retrieval, assembly/TSS filters, binding/context ranking, and separation from de novo inference. |
| `coexpression-network` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_4aec92a664ad4b63baaf2b4981daa277?section=marketplace | 2026-08-14T12:19:43.688Z | `When to Use This Skill`; `Standard Workflow`; `Weighted Gene Co-expression Network Analysis (WGCNA)`; `Outputs`; `Common Issues`; `Interpretation Guidelines`. Retains sample-size gate, variable-gene/soft-power selection, eigengenes, module-trait tests, hub ranking, and association-not-causality limit. |
| `upstream-regulator-analysis` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_cf9489278b984eef91e08a40b68943be?section=marketplace | 2026-08-14T12:23:27.669Z | `When to Use This Skill`; `Inputs`; `Standard Workflow`; `Upstream Regulator Analysis`; `Direction Classification`; `Regulatory Score`; `Key Caveats`; `Common Issues`. Retains separate up/down enrichment, overlap testing, concordance classification, heuristic scoring, and disclosure of coverage bias. |

## Rule-to-source map

- De novo GRN stages, cell/gene gates, and regulon evidence products ->
  `grn-pyscenic.yaml`.
- Public TF target retrieval and assembly/TSS constraints ->
  `chip-atlas-target-genes.yaml`.
- Coexpression sample-size, module, hub, and scale-free diagnostics ->
  `coexpression-network.yaml`.
- Upstream enrichment, Fisher overlap, direction classes, and heuristic score
  -> `upstream-regulator-analysis.yaml`.

All source timestamps are preserved exactly from normalized `source` records.
