# Provenance

This portable router is derived from four archived bulk-omics source modes.
Raw captures preserve the visible source text; normalized YAMLs provide the
structured rule and provenance fields used for distillation.

## Archive inputs

| Source skill | Normalized record | Raw visible source | Raw metadata |
|---|---|---|---|
| bulk-omics-clustering | `../../02_normalized/bulk-omics-clustering.yaml` | `../../00_raw/bulk-omics-clustering/source.md` | `../../00_raw/bulk-omics-clustering/metadata.json` |
| bulk-rnaseq-counts-to-de-deseq2 | `../../02_normalized/bulk-rnaseq-counts-to-de-deseq2.yaml` | `../../00_raw/bulk-rnaseq-counts-to-de-deseq2/source.md` | `../../00_raw/bulk-rnaseq-counts-to-de-deseq2/metadata.json` |
| deconvolution-bulk-rnaseq | `../../02_normalized/deconvolution-bulk-rnaseq.yaml` | `../../00_raw/deconvolution-bulk-rnaseq/source.md` | `../../00_raw/deconvolution-bulk-rnaseq/metadata.json` |
| rnaseq-fastq-to-counts | `../../02_normalized/rnaseq-fastq-to-counts.yaml` | `../../00_raw/rnaseq-fastq-to-counts/source.md` | `../../00_raw/rnaseq-fastq-to-counts/metadata.json` |

## Derived-rule mappings

| Derived mode/rule | Source skill | Visible source section | Source URL | Retrieved at |
|---|---|---|---|---|
| Make matrix orientation, normalization, missingness, batches, and geometry explicit before clustering | bulk-omics-clustering | `Standard Workflow`; `Decision Guide`; `Inputs` | https://biomni.phylo.bio/skills/skill_51a601b71a454a00aa0d378fe8e3205c?section=marketplace | 2026-08-14T12:17:14.93Z |
| Triangulate cluster quality with metrics, stability, labels, and domain plausibility | bulk-omics-clustering | `Decision Guide`; `Common Issues`; `Outputs` | https://biomni.phylo.bio/skills/skill_51a601b71a454a00aa0d378fe8e3205c?section=marketplace | 2026-08-14T12:17:14.93Z |
| Keep bulk clustering separate from single-cell clustering and co-expression inference | bulk-omics-clustering | `When to Use This Skill`; `Scientific caveats` | https://biomni.phylo.bio/skills/skill_51a601b71a454a00aa0d378fe8e3205c?section=marketplace | 2026-08-14T12:17:14.93Z |
| Require raw integer counts, aligned metadata, and a non-confounded design for count DE | bulk-rnaseq-counts-to-de-deseq2 | `Inputs`; `Standard Workflow`; `Design Formulas` | https://biomni.phylo.bio/skills/skill_019caa158c8a4011b19285967639a364?section=marketplace | 2026-08-14T12:17:24.692Z |
| Separate shrunken ranking effects from unshrunk testing estimates and inspect PCA/MA/dispersion | bulk-rnaseq-counts-to-de-deseq2 | `Log Fold Change Shrinkage`; `Quality Control`; `Decision Points` | https://biomni.phylo.bio/skills/skill_019caa158c8a4011b19285967639a364?section=marketplace | 2026-08-14T12:17:24.692Z |
| Route normalized inputs or very small groups to an appropriate alternative and disclose it | bulk-rnaseq-counts-to-de-deseq2 | `Common Issues`; `When Scripts Fail`; `Scientific caveats` | https://biomni.phylo.bio/skills/skill_019caa158c8a4011b19285967639a364?section=marketplace | 2026-08-14T12:17:24.692Z |
| Harmonize bulk/reference identifiers and scale, use a multi-method consensus, and retain concordance | deconvolution-bulk-rnaseq | `Standard Workflow (R)`; `Inputs`; `Validation Scope` | https://biomni.phylo.bio/skills/skill_9220a3a4f3fd3052b91800e13683d7cf?section=marketplace | 2026-08-14T12:20:10.892Z |
| Treat missing reference populations and method-fragile cell types as explicit limitations | deconvolution-bulk-rnaseq | `When to Use This Skill`; `Common Issues`; `Validation Scope` | https://biomni.phylo.bio/skills/skill_9220a3a4f3fd3052b91800e13683d7cf?section=marketplace | 2026-08-14T12:20:10.892Z |
| Keep synthetic recovery separate from independent accuracy validation | deconvolution-bulk-rnaseq | `Validation Scope`; `Outputs` | https://biomni.phylo.bio/skills/skill_9220a3a4f3fd3052b91800e13683d7cf?section=marketplace | 2026-08-14T12:20:10.892Z |
| Validate FASTQ/reference compatibility, strandedness, one count engine, and DE-readiness | rnaseq-fastq-to-counts | `Workflow`; `Scope`; `Scientific caveats` | https://biomni.phylo.bio/skills/skill_b6b5b3b7da495f7833229bedd699bc7a?section=marketplace | 2026-08-14T12:16:47.164Z |
| Set sjdbOverhang to read length minus one and avoid mixing STAR and Salmon quantities | rnaseq-fastq-to-counts | `Workflow`; `Inputs`; `Outputs (saved under /mnt/results/<run>/)` | https://biomni.phylo.bio/skills/skill_b6b5b3b7da495f7833229bedd699bc7a?section=marketplace | 2026-08-14T12:16:47.164Z |
| Require replicates and label subset-mode counts as validation rather than biological evidence | rnaseq-fastq-to-counts | `Downstream hand-off (put in README.md)`; `Scientific caveats` | https://biomni.phylo.bio/skills/skill_b6b5b3b7da495f7833229bedd699bc7a?section=marketplace | 2026-08-14T12:16:47.164Z |

## Resource and exclusion notes

The resource catalog is `../../03_resource_registry/resource_registry.yaml`
(registry version `2026-08-14`, 1,161 final unique records). Reference and
software names remain optional adapters. Source-local scripts, environment
paths, orchestration, and report packaging were excluded.
