# Provenance

Three source workflows are combined as a staged router. Their evidence stages
remain separate: calling, annotation, and clinical allelic-series/actionability.

| Source skill | Normalized record | Raw visible source | Source URL | Retrieved at | Sections and rules retained |
|---|---|---|---|---|---|
| `variant-calling-from-sequencing` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_21c4aae0e24ee315b4095239a58ceb6c?section=marketplace | 2026-08-14T12:16:36.319Z | `Scope`; `Inputs`; `Workflow`; `Outputs`; `Scientific caveats (read before interpreting results)`; `Command reference (verified working patterns)`. Retains sample/reference/build contract, coverage and call QC, caller configuration, truth/replicate checks, and call-level provenance. |
| `genetic-variant-annotation` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_caf61714cb394d14b2d099387baec0b0?section=marketplace | 2026-08-14T12:18:21.752Z | `When to Use This Skill`; `Inputs`; `Standard Workflow`; `Genetic Variant Annotation`; `Common Issues`; `Report Packaging`; `Primary Citations`; `Outputs`. Retains allele normalization, build/transcript policy, consequence and frequency annotation, missing/conflict handling, and evidence-version provenance. |
| `clinical-variant-allelic-series` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_71ac25ccb32e00a971a5e49bcae1bfe2?section=marketplace | 2026-08-14T12:18:38.467Z | `Scope`; `Inputs`; `Workflow steps (and why each matters)`; `Clinical Variant Allelic Series`; `Database reference`; `Scientific caveats & hard-won lessons`; `Error handling / graceful degradation`; `Outputs (written to --outdir)`. Retains position-resolved clinical evidence, phenotype/context joins, evidence tiers, ClinVar/CIViC-like source separation, and conservative actionability claims. |

## Rule-to-source map

- Sample/QC/caller/build gates -> `variant-calling-from-sequencing.yaml`.
- Allele normalization, consequence/frequency annotation, and build/version
  provenance -> `genetic-variant-annotation.yaml`.
- Clinical evidence, allelic series, phenotype fit, and actionability tiers ->
  `clinical-variant-allelic-series.yaml`.

URLs, timestamps, and raw/normalized paths are copied from the normalized
`source` records. Resource access and current evidence status require a fresh
runtime check.
