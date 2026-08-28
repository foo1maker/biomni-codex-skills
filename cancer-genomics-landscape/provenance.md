# Provenance

## Archive input

| Source skill | Normalized record | Raw visible source | Raw metadata |
|---|---|---|---|
| cancer-cohort-genomics | `../../02_normalized/cancer-cohort-genomics.yaml` | `../../00_raw/cancer-cohort-genomics/source.md` | `../../00_raw/cancer-cohort-genomics/metadata.json` |

## Derived-rule mappings

| Derived rule/mode | Source section | Source URL | Retrieved at |
|---|---|---|---|
| Resolve genes/cohorts and retain assay/profile scope | `Inputs`; `Workflow` | https://biomni.phylo.bio/skills/skill_861901b9194211e7a334b4f005209808?section=marketplace | 2026-08-14T12:17:35.436Z |
| Use the intersection of sequenced and copy-number-profiled samples as the combined denominator | `Workflow`; `Acceptance checks (run before declaring done)` | https://biomni.phylo.bio/skills/skill_861901b9194211e7a334b4f005209808?section=marketplace | 2026-08-14T12:17:35.436Z |
| Keep missing assays unavailable rather than imputing zero | `Workflow`; `Common pitfalls` | https://biomni.phylo.bio/skills/skill_861901b9194211e7a334b4f005209808?section=marketplace | 2026-08-14T12:17:35.436Z |
| Switch between recurrent-hotspot and dispersed-allele reporting | `Workflow`; `Outputs (to /mnt/results/)` | https://biomni.phylo.bio/skills/skill_861901b9194211e7a334b4f005209808?section=marketplace | 2026-08-14T12:17:35.436Z |
| Preserve denominator, frequency-range, row-count, and figure/report checks | `Acceptance checks (run before declaring done)` | https://biomni.phylo.bio/skills/skill_861901b9194211e7a334b4f005209808?section=marketplace | 2026-08-14T12:17:35.436Z |
| Interpret pooled cohort differences as case-mix-sensitive descriptions, not causal biology | `Scientific caveats (state these in the report)` | https://biomni.phylo.bio/skills/skill_861901b9194211e7a334b4f005209808?section=marketplace | 2026-08-14T12:17:35.436Z |

## Resource and exclusion notes

The catalog authority is `../../03_resource_registry/resource_registry.yaml`
(registry version `2026-08-14`, 1,161 final unique records). cBioPortal, TCGA
open access, and UCSC are replaceable adapters; access, build, and licensing
must be checked before use. Variant calling, structural variants, fusions,
survival, and platform report packaging were outside this derived contract.
