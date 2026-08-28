# Provenance

This skill is a direct portable mapping of the archived spatial transcriptomics
workflow. Platform-specific report and script shells are not part of the
contract.

| Source skill | Normalized record | Raw visible source | Source URL | Retrieved at | Sections and rules retained |
|---|---|---|---|---|---|
| `spatial-transcriptomics` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_30b65ae0dee54a929017fbc760aefe01?section=marketplace | 2026-08-14T12:21:06.913Z | `When to Use This Skill`; `Inputs`; `Standard Workflow`; `Outputs`; `Agent Summary Guidelines`; `Common Issues`; `Interpreting Results`; `References`. Retains coordinate/image alignment, platform-specific assumptions, QC, domains, variable genes, neighborhood enrichment, replicate handling, and spatial-resolution limits. |

## Rule-to-source map

- Count/coordinate/image input contract -> `Inputs`.
- QC, normalization, domains, variable genes, and neighborhood workflow ->
  `Standard Workflow`.
- Alignment, replicate, and figure integrity gates -> `Quality Control` and
  `Common Issues`.
- Spot/cell resolution and localization interpretation limits ->
  `Interpretation Guidelines`.

The URL, timestamp, and raw/normalized paths are copied from the normalized
`source` record.
